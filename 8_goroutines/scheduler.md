## Scheduler Mechanics

A definição simples para Concurrency é a execução de operações em sequência (e talvez fora de ordem). 
A definição simples de Paralelism é a execução de operações ao mesmo tempo.

A capacidade de paralelismo ou concorrência em aplicações depende do hardware e de um "path de execução", como a(s) thread(s) de um processador ou de um sistema operacional, onde um sistema single thread não tem a capacidade de execução de intruções em paralelo e, portanto, um sistema multi threads sim.

Ver `./goroutines/README.md` para exemplo de scheduler em Go.
