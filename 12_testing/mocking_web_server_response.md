### Mocking web server response

Análise de `./tests/example3`. Utiliza o package `httptest`, útil quando a saída para o host real não é necessária e para simular falhas no retorno que não aconteceriam em um sistema produtivo.

Compare os tempos dos exemplos anteriores com este.
````
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

A unit test is a test of behavior whose success or failure is wholly determined by the correctness of the test and the correctness of the unit under test ~ Kevin Henney