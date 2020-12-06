### IO-Bound benchmarking

Análise de `./benchmark/io-bound`. Geror de 10k documentos.  
Desligar o GC durante o benchmark é uma possibilidade uma vez que ele gera latência. A motivação aqui é validar o benchmark evitanto qualquer interferência que ele possa sofrer em tempo de CPU.  
Nesse tipo de teste é interessante observar os resultados alterando hardware threads (CPU thread) com `-cpu`, além de aumentar o tempo default do teste de 1s para 3s.

`Concurrent` tem melhor performance mesmo com apenas 1 cpu. Isso prova que para aumentar o trhougput não é preciso aumetar o paralelismo em um modelo concorrente quando o workload é IO-Bound, diferente de quando o workload é CPU-Bound.
```
GOGC=off go test -cpu 1 -run none -bench . -benchtime 3s
Processing 10000000 numbers using 4 goroutines
goos: darwin
goarch: amd64
pkg: github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/cpu-bound
BenchmarkSequential                  432          10407280 ns/op
BenchmarkConcurrent                  397           9500326 ns/op
BenchmarkSequentialAgain             422          10152456 ns/op
BenchmarkConcurrentAgain             418           9071629 ns/op
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/cpu-bound  20.193s
``` 