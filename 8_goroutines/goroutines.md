## Go Routines

- Sincronização: go routines estão sendo executadas separadamente em fila
- Osquestração: há interação entre 2+ go routines

Em `./goroutines/example1` um WaitGroup é utilizado para exemplo de conorrência de um algoritmo single thread (vide GOMAXPROCS que define o limite de theads em 1). Ao executar o programa podemos ver que a segunda goroutine termina primeiro, chegando até o seu final para que a segunda tome ação. O algotimo está garantindo concorrência porém não ordem de execução. Quando ordem importa concorrência não deve ser utilizada mas sim garantias de execução (nesse exemplo com WaitGroup).

Em `./goroutines/example2` mostra o número de context switches das go routines.

Em `./goroutines/example3` provome o algoritmo a execução em paralelo das go routines (GOMAXPROCS(2)) mantendo o WaitGroup para que ambas sejam executadas. Em função desse comportamento, diferente do exemplo 1, vemos a saída das funções misturadas no console.