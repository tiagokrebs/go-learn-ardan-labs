## Error handling

Systems cannot be developed assuming that human beings will be able to write millions of lines of code without making mistakes, and debbuging alone is not an efficient way to develop reliable systems - Al Aho (AWK)

### Default error values
Análise de `error_handling/example1/example1.go`. Error é uma interface builtin com valores string não exportados.
Nesse exemplo a function `WebCall()` retorna uma nova string através da interface Error enquanto a main function faz comparação do resultado da chamada com `nil`. Em Go valores não definidos a variáveis são determinados pelo zero value do type, para uma interface o esse valor é `nil`. Quando a comparação identifica um valor concreto em `err` a string do erro é exibida.

### Error variables
Análise de `error_handling/example2/example2.go`. 
A próxima escolha lógica quando apenas saber que há um erro não é o bastante pode ser o uso de variáveis de erro. Normalmente essas variáveis são separadas em um bloco `var()` no início do código ou importadas de um package. Nesse caso o switch com default garante a identificação dos tipos de erros e outros casos sem tipo definido.

### Types as context
Análise de `error_handling/example3/example3.go`.
Nesse exemplo a condição na main funciotion não é mais baseada no valor mas no type do valor armazenada na interface Error, type como contexto.
Um problema dessa abrodagem é que estamos saindo de um decouplig e voltando para o dado concreto. Um dos guidelines mais básicos em Go é manter o nível de decouplig proporcionado pelas interfaces sempre que possível. O pacote JSON buitin é um bom exemplo desse caso de uso.

### Behavior as context
Análise de `error_handling/example4/example4.go`.
Exemplo de utilização da interface `temporary` do Pacote NET.

### Find the bug
Análise de `error_handling/example5/example5.go`.

### Wrapping errors
Análise de `error_handling/example6/example6.go`.
Fazer error handling no código não é uma tarefa simples e deve ter algumas regras, como por exemplo:
- Os erro não deve ser propagado pela API mas parar no código onde ele está sendo tratado. 
- Sempre a partir de um erro deve acontecer um recovery ou shutdown não administrados pela fonte geradora do erro mas em outra parte do call stack da aplicação, decididos previamente no source.
- Erros sempre devem ir para logs, com o contexto completo.

`Wrapf` é a forma de retornar os erros para cima no call stack até o ponto onde serão tratados utilizando `Cause`, que nesse exemplo nos permite percorrer todo o contexto de erros retornados pelas chamadas até chegar no valor no primeiro wrap efetuado na function `thirdCall()`.
Essa forma de utilização permite o uso de formatos especiais no pacote `fmt` para print do stack completo do erro. Nice!