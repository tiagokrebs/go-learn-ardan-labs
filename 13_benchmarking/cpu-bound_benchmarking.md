### CPU-Bound benchmarking

Análise de `./benchmark/cpu-bound`. Gerador de 1mi de números, sequencialmente e concorrente (1 goroutine por CPU).  
Desligar o GC durante o benchmark é uma possibilidade uma vez que ele gera latência. A motivação aqui é validar o benchmark evitanto qualquer interferência que ele possa sofrer em tempo de CPU.  
Nesse tipo de teste é interessante observar os resultados alterando hardware threads (CPU thread) com `-cpu`, além de aumentar o tempo default do teste de 1s para 3s.

```
GOGC=off go test -run none -bench . -cpu 1 -benchtime 3s
Processing 10000000 numbers using 4 goroutines
goos: darwin
goarch: amd64
pkg: github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/cpu-bound
BenchmarkSequential                  486           7266480 ns/op
BenchmarkConcurrent                  490           7552880 ns/op
BenchmarkSequentialAgain             498           7058541 ns/op
BenchmarkConcurrentAgain             499           7215944 ns/op
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/cpu-bound  17.625s
```
`Sequential` tem melhor performance com apenas 1 cpu. `Concurrent` está instanciando N goroutines (N = quantidade de CPUs) porém executando com apenas 1 cpu definido no benchmark, logo o número de interrupções degrada a performance. Permitindo ao benchmark utilizar todas as threads disponíveis do CPU o tempo de `Concurrent` é menor, podemos dizer que o seu o trhoughput é maior.
