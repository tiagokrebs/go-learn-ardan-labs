### Basic unit testing

**Do you really need a third-party package for this? I mean REALLY? Are you sure?**  
Manter a consistência na escrita dos testes no time é importante, porém isso não depende te um pacote de terceiros e sim de métodos e guidelines bem definidos. Por isso pacotes de terceiros para testes não são indicados nos guidelines desse projeto


Análise de `./tests/example1`.  
A convenção para que a ferramenta de teste buil-in reconheça um arquivo de teste é a string `_test` no nome do arquivo. Também, as funções devem ser exportatas utilizando `Test` no início do nome, além do ponteiro `*testing.T`.  
`*testing.T.` contém uma API composta por `t.Logf` (verbose simples), `t.Errorf` (falha sem interrupção nos testes), `t.Fatalf` (falha com interrupção nos testes).  
```
go test example1_test.go -v
=== RUN   TestDownload
    TestDownload: example1_test.go:20: Given the need to test downloading content.
    TestDownload: example1_test.go:22:  Test 0: When checking "https://www.ardanlabs.com/blog/index.xml" for status code 200
    TestDownload: example1_test.go:28:  ✓       Should be able to make the Get call.
    TestDownload: example1_test.go:33:  ✓       Should receive a 200 status code.
--- PASS: TestDownload (0.85s)
PASS
ok      command-line-arguments  0.875s
```

Para múltiplos arquivos de teste com filtro.
```
go test -run Down -v
=== RUN   TestDownload
    TestDownload: example1_test.go:20: Given the need to test downloading content.
    TestDownload: example1_test.go:22:  Test 0: When checking "https://www.ardanlabs.com/blog/index.xml" for status code 200
    TestDownload: example1_test.go:28:  ✓       Should be able to make the Get call.
    TestDownload: example1_test.go:33:  ✓       Should receive a 200 status code.
--- PASS: TestDownload (1.16s)
PASS
ok      _/Users/tiago.krebs/Projects/golang/go-learn-ardan-labs/12_testing/tests/example1       1.173s
```

