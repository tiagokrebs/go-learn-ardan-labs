### Validate benchmarking

Análise de `./benchmarks/validade`. Benchmark de algoritmo merge sort utilizando de uma a infinitas goroutines.  
```
go test -run none -bench . 
goos: darwin
goarch: amd64
pkg: github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/validate
BenchmarkSingle-4              9         126741651 ns/op
BenchmarkUnlimited-4           1        1827745291 ns/op
BenchmarkNumCPU-4              5         205651368 ns/op
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/validate   6.312s
```

Como executar esse algoritmo pode ser mais rápido utilizando apenas 1 goroutine do que múltiplas em paralelo? Não pode. Ao executar o benchmark de três funções diferentes estamos gerando muitas interrupções na CPU, o que vai mascarar o resultado dos testes. Executando o benchmark de cada função separadamente `BenchmarkNumCPU` se mostra a mais performática (1 goroutine por CPU core).  
Não é necessário sempre executar benchmarks sequencialmente, porém a carga gerada na CPU por múltiplos testes deve ser considerada caso ela possa se tornar um fator de interferência nos resultados.