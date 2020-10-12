### Pooling

Análise de `./channels/example1`. As N goroutines disparadas na linha 119 iniciam bloqueadas na linha 120 aguardando sinais no channel.
Em pooling o channel não deve ser bufferizado devido a perda de controle sobre a confirmação do signal.