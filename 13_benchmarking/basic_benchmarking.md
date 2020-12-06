### Basic benchmarking

- Benchmarks são para CPU e memória.  
- Tem latência maior em funções das interrupções que durante a execução do programa para coletar as medidas
- Como o teste de bechmark acontece a partir de um binário gerado pelo compilado go Go é preciso levar em consideração otimizações que podem afetar os resultados. O código deve ser o mais fiel possível ao código de produção. Atentar para código que o compilador pode abstrair.

Análise de `./benchmarks/basic`.  
A convenção para que a ferramenta de benchmark buil-in reconheça um arquivo de teste é a string `_test` no nome do arquivo. Também, as funções devem ser exportatas utilizando `Benchmark` no início do nome, além do ponteiro `*testing.B`.  

Analisando as duas funções deesse exemplo `BenchmarkSprintf` mostra um desempenho melhor. Para ignorar possíveis testes e executar apenas benchmarks utilizamos `-run none`.
```
go test -run none -bench . -benchmem
goos: darwin
goarch: amd64
pkg: github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/basic
BenchmarkSprint-4       10753820               101 ns/op               5 B/op          1 allocs/op
BenchmarkSprintf-4      12133581                88.5 ns/op             5 B/op          1 allocs/op
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/benchmarks/basic      2.380s
```

Análise de Análise de `./benchmarks/sub`. Assim como os testes podem possuir sub-tests benchmarks também podem possuir sub-benchmarks (ver `12_testing/sub_tests`).
