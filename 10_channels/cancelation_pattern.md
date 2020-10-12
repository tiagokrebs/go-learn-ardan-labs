### Cancelation pattern

Análise de `./channels/example1`. Cancelation pattern é sobre a implementação de timeouts para operações. O uso de context é aplicado para eesse tipo de operação.
Na linha 262 um channeel bufferizado com apenas 1 slot é criado enquando na linha 264 uma goroutine é criada para interagir com o channel.
No select da linha 269, como não há `default` dois diferentes sinais são aguardados: o sinal da goroutine ou o sinal do contexto (ctx.Done().
O que ctx.Done() é o sinal de timeout do context definido na linha 259, caso esse sinal seja recebido antes do sinal da goroutine o select sairá do estado bloqueado, permitindo que o programa principal continue e eventualmente chegue ao fim.

Um channel não bufferizado não pode ser aplicado a esse pattern devido a sua característica de confirmação do sinal. Caso o programa principal já tenha saído do select devido ao timeout do contexto no momento em que a gorotine tentar enviar o sinal não haverá mais um receiver para o sinal (definido pelo unbuffer channel), isso gera uma goroutine leak.