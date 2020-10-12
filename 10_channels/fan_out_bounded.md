### Fan-ou bounded

Análise de `./channels/example1`. Exemplo com uma quantidade discreta de tasks a serem executadas porém sem a necessidade de disparara uma goroutine para cada.
O pattern de pooling é utilizado com a adição de um `WaitGroup`, ambos, o tamanho do buffer no channel e o count do WaitGroup, com seus limites definidos de acordo com o número máximo de goroutines que podem estar running em paralelo (runtime.NumCPU).
Na linha 205 o programa principal é bloqueado até que uma goroutine termine o seu trabalho (wg.Done) permitirndo a criação de uma nova goroutine runnable que imediatamente entra em estado running.
Imediatamente após todas as goroutines (2000) serem disparados o channel é fechado na linha 207, isso não gera o cancelamento imediato das últimas goroutines em execução. A linha 208 (wg.Wait) garante que o programa principal aguarde as últimas goroutines disparadas terminem o seu trabalho antes do final.