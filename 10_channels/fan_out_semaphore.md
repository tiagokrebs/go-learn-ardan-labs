### Fan-out semaphore

Análise de `./channels/example1`. Na linha 150 um channel bufferizado com 2000 slots (1 para cada goroutine) é criado, dessa forma o send pode ser concluído sem aguarardar o sinal de receive.
Na linha 153 outro channel bufferizado é criado porém com a quantidade dee slots definida de acordo com as goroutines que estarão em running (runtime.NumCPU).
Na linha 156 as 2000 goroutines não criadas em estado runnable, enquanto na linha 157 cada goroutine que entrar em estado running envia um sinal para o channel auxiliar (sem).
Sabemos que apenas runtime.NumCPU goroutines podem ser executadas em paralelo, logo quando esse número de goroutines enviar um sinal para `sem` (linha 157) o channel vai bloquear até que todas as goroutines terminem. O final na linha 163 é ação da goroutine retirando o seu valor do channel auxiliar, o que permite novamente a execução de uma nova goroutine runnable.
Esse tipo de channle auxiliar também é chamado se semi-for.