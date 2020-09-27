# Data Races

Diferentes go routines tentando escrever ou escrever e ler a mesma localização de memória ao mesmo tempo, nasty bug em multithread software.

Análise de `./data_race/example1`, exemplo de race condition. `counter` é um estado compartilhado por duas goroutines que fazem a leitura e escrita na mesma posição de memória. Um `WaitGroup` foi utilizado com a intenção de garantir sincronia entre elas e inicialmente o programa parece funcionar normalmente, retornando sempre o valor: 4. Mas, ao adicionar uma finstrução que permite ao compilador executar um context switch (`Println` linha 39) um data race acontece e o retorno do programa muda (argh). 

Para evitar esse tipo de comportamento é possível utilizar do detector de race condition, compilando o programa com a flag `-race`. Essa flag deve ser usada apenas para hambientes não produtivos com o objetivo de teste ou debug. Devido a característica do programa compilado dessa forma a latência do mesmo pode aumentar em até 25%.

O retorno do detector de race condition indica o stack de onde ele está acontecedo.
```
% ./example1    
logging
logging
logging
==================
WARNING: DATA RACE
Write at 0x000001283580 by goroutine 8:
  main.main.func1()
      /Users/tiago.krebs/Projects/golang/go-learn-ardan-labs/9_data_races/data_race/example1/example1.go:42 +0xce

Previous read at 0x000001283580 by goroutine 7:
  main.main.func1()
      /Users/tiago.krebs/Projects/golang/go-learn-ardan-labs/9_data_races/data_race/example1/example1.go:33 +0x4a

Goroutine 8 (running) created at:
  main.main()
      /Users/tiago.krebs/Projects/golang/go-learn-ardan-labs/9_data_races/data_race/example1/example1.go:29 +0xab

Goroutine 7 (finished) created at:
  main.main()
      /Users/tiago.krebs/Projects/golang/go-learn-ardan-labs/9_data_races/data_race/example1/example1.go:29 +0xab
==================
logging
Final Counter: 2
Found 1 data race(s)
```


Análise de `./data_race/example2`. Utilizar `atomic` é uma das maneiras mais rápidas de evitar um race condition porque elas acontecem a nível de hardware. Um ponto contra desse comportamento é que o hardware normalmente consegue processar apenas 1 word ou 0.5 word de cada vez para esse tipo de operação. Nesse exemplo como o estado é um inteiro que, dependendo da arquitetura, tem o tamanho de 1 word ou 0.5 word o uso de `atomic` é viável. Isto posto, pode se dizer que esse tipo de instrução é ideal para contadores.
Quando usamos `atomic` é necessário ser explícito quanto a precisão dos inteiros. Na linha 15 instanciamos `var counter int64` e na linha 30 estamos executando `AddInt64`. Quando não explicitarmos a precisão da variável o compilados decide de acordo com a arquitetura, o que pode levar a uma diferença de precisão e logo, uma exceção.

Comparado com o `exemplo1` podemos ver que apenas a instrução para adição de valor ao contador pode ser garantida com o uso de `atomic`. Quando compilado com `-race` o detector não encontra mais race conditions no `exemplo2`. 


Análise de `./data_race/example3`. Se um bloco de código necessita da garantia de sincronia para evitar um data race, precisamos fazer uso de um `Mutex`. Essa API nos permite um bloco de código se comporte de forma atômica.
Mutexes controlam a execução fazendo `Lock` e `Unlock` das goroutines, logo, são tarefas essenciais. Uma goroutine sem `Unlock()` vai gerar o deadlock de todas as demais. Outra característica importante é que `Mutex` não é uma fila, potencialmente goroutines disparadas potencialmente podem ser executadas antes mesmo quando iniciadas depois de outras goroutines que já estão aguardando `Unlock()` acontecer.

Uma boa prática é criar um bloco de código artifical entre lock/unlock para melhor visualização das instruções.

O maior possível problema no uso de Mutexes é o back-pressure que ele cria. Quanto mais tempo o bloco de instruções entre lock/unlock tomar na CPU maior será a latência adicionada ao programa.


Análise de `./data_race/example4`. Mutexes podem assumir uma característica única de leitura ou escrita o que nos permite múltiplos readers uma vez que é o processo de mutação/escrita o gerador das race conditions. 