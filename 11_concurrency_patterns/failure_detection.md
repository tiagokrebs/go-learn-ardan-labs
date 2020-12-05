### Failure detection

Análise de `./example`. Caso de uso de software com muita escrita de log com deadlock devido a espaço em disco insuficiente.

Linhas 16-31 de main.go simula o comportamento do espaço em disco.  
Linhas 57-58 permitem ^C signal para fazer o switch da simulação do espaço em disco (toogle). Como 10 goroutines são disparadas esse switch contém um data race, porém ele é ignorado por ser apenas uma ferramenta da simulação proposta. O switch muda o programa entre loop infinito e escrita infinita de logs.

As goroutines que fazem a escrita do log não podem sofrer block devido a falta de espaço em disco (simulada com ^C). Podemos pensar nesse problema como um padrão cássico de drop da operação de escrita, evitando o block.

**Solução proposta:**  
Escrever um novo logger package. Nesse exemplo não temos a capacidade para atender escrita em disco de dezenas de milhares goroutines. Através do novo packge reduzimnos a escrita para apenas 1 goroutine, facilitando a detecção quando há um problema na escrita.  
Devido a característica de múltiplas goroutines do programa utilizando apenas 1 goroutine para escrita do log devemos utilizar signaling através de um buffer channel (fan-in).  
Monitorando a capacidade do channel através do buffer é possível criar um gap de 20%, onde ao atingir 80% o package fará a escrita em disco. A partir do momento em que o package logger receber block pela falta de espaço no dispositivo o gap de 20% restante vai ser preenchido, é nesse momento que o logger faz drop da escrita.  
Dessa forma as goroutines que dependem da escrita em log não serão bloqueadas.

Veja `./logger/logger.go`.