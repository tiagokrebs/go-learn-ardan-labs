### Fan-out

Análise de `./channels/example1`. Em um ponto da execução principal sinais são esperados em um channel no qual N goroutines estão signaling. O programa principal aguarda todos os sinais enviados para o channel antes de prosseguir. Linha 68 dispara 2000 goroutines das quais um signal é esperado.