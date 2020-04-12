## Arrays

Arrays são uma das poucas estruturas de dados disponíveis em Go. A falta de outras estruturas é justificada principalmente devido a natureza do array ter *mechanical sympathy* com o hardware.

### Mechanical Sympathy
O exemplo em `./caching` demostra a criação de uma matriz de de uma lista encadeada utilizando slice e métodos para atravesar essas duas estruturas. Executando o teste de benchmark abaixo é possível identificar o desempenho dos métodos para cada estrutura do programa

```
$ go test -run none -bench . -benchtime 3s
Elements in the link list 16777216
Elements in the matrix 16777216
goos: linux
goarch: amd64
BenchmarkLinkListTraverse-4          100          31467498 ns/op
BenchmarkColumnTraverse-4             25         141859337 ns/op
BenchmarkRowTraverse-4               147          23091423 ns/op
PASS
ok      _/home/krebs/Projetos/golang/Ultimate-Go-Ardan-Labs/3_data_structures/caching    21.670s
```

*BenchmarkColumnTraverse* é mais lento devido a taxa de TLB MISS (a informação está alocada em diferentes páginas do SO). Devido ao tamanho de 1024 da linha cada coluna acaba sendo alocada em diferentes blocos de memória nas camadas de cache do processador, fazendo com que o acesso a informação necessite de mais istruções e tornando o processo mais lento. Por óbvio esse comportamento varia de acordo com o hardware sob o qual o teste é aplicado em função dos diferentes tamanho de memória l1, l2, l3, TLB e clock do processador. Podemos chamar esse tipo de diferentes comportamento de *simpatia mecânica*.

Go é uma linguagem em que o hardware é o modelo, deferente de outras linguagens que podem reorganizar o código para que tenham mais simpatia meânica como Java ou C#.
Estruturas de dados como array, slice (array dinâmico) e map são implementados em Go devido a grande simpatia mecânica com o hardware.

### Semantics
Guideline:  
- Quando usamos builtin types no programa é preferível utilizar *value semantics* (cópia da informação no stack) devido ao comportamento do compilador que utiliza *pointer semantics* quando necessário para otimizar execução (string).
- Quando contruimos types ou structs *pointer semantics* são utilizados de acordo com a necessiadade de otimização do uso de memória nos stacks e heap (GC).


### Range Mechanics
A iteração utilizando *range* também segue os modelos *pointer semantics* ou *value semantics*. O programa `./arrays/example4/example4.go` exemplifica o uso de cada um.

