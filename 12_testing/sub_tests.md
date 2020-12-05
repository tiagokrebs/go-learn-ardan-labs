### Sub tests

Análise de `./tests/example5`. Criação de literal function com bind nos nomes na table unit para serem executados como sub testes (ex: filtrando por nome).

```
go test -run TestDownload/statusok -v      
=== RUN   TestDownload
    TestDownload: example5_test.go:32: Given the need to test downloading different content.
=== RUN   TestDownload/statusok
    TestDownload/statusok: example5_test.go:36:         Test: 0 When checking "https://www.ardanlabs.com/blog/index.xml" for status code 200
    TestDownload/statusok: example5_test.go:42:         ✓       Should be able to make the Get call.
    TestDownload/statusok: example5_test.go:47:         ✓       Should receive a 200 status code.
--- PASS: TestDownload (0.64s)
    --- PASS: TestDownload/statusok (0.64s)
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/tests/example5        0.655s
```

Sub tests também adicionam a possibilidade de execução de múltiplos testes em paralelo, function `TestParallelize`. Compare os tempos da execução sequencial e em paralelo.

```
% go test -run TestDownload      
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/tests/example5        0.893s

go test -run TestParallelize   
PASS
ok      github.com/ardanlabs/gotraining/topics/go/testing/tests/example5        0.325s
```