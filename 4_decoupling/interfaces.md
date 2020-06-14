## Interfaces

Types interface não são types concretos, eles não definem nenhum tipo de dado mas sim um conjunto de métodos de comportamento. Mesmo assim uma interface pode ser definida da seguinte forma:
```
var r reader // reader é uma interface builtin
```
`r` é uma variável que não tem nem pode assumir nenhum valor logo essa forma de declaração deve seer evitada.

### Polymorphysm
> Polymorphysm mean that you write a certain program and it behaves differently depending on the data that it operates on ~ Tom Kurts (inverntor do BASIC)

Análise de `./interfaces/example1/example1.go`. 
Métodos `read` de file e pipe utilizam *value semantics*.


### Method Sets & Address of Value
Análise de `./interfaces/example2/example2.go`.
```
sendNotification(u)
// ./example1.go:36: cannot use u (type user) as type notifier in argument to sendNotification:
//   user does not implement notifier (notify method has pointer receiver)
```
Spec define method sets para prevenir perda massiva de integridade no programa ao utilizar o data semantics como base. O exemplo acima é um desses casos.

Regra 1 - Se a informação utilizada é um valor de algum tipo então apemas os métodos implementados utilizando value receivers serão ligados naquela informação; *value semantic*.  
Regra 2 - Se a informação utilizada é um endereço (ponteiro), tando métodos que utilizam *pointer semantics* quando *value semantics* poderão ser chamados.

A interface em `exeplo2.go` foi definida utilizado *pointer semantic* 
```
func (u *user) notify() {...}
```
mas quando fazemos a chamada de notify
```
func sendNotification(n notifier) {...}
```
estamos utilizando *value semantics*, a qual o o compilador não incluiu o método utilizando *pointer semantics* em função da Regra 1 acima.

### Storage by Value
Análise de `./interfaces/example4/example4.go`.
```
entities := []printer{...}  // collection of interface
```
Devido a característica do data semantic da struct `user` apenas a instrução utilizando *pointer smantics* (linha 38) vai receber a alteração (linha 42). Ao utilizar a função `Printf` que utiliza *value semantics* sobre a *collection* de `user` é possível averiguar isso.

Uma fora para justificar o uso da forma de *value semantics* no `range` (linha 46) nesse caso é seguindo os types utilizados e o data semantic de cada um:
* O que são entities São collections;
* Colletion de que? Printer;
* O que é Printer no source? Um interface;
* O que é uma interface? É um reference type;

O guideline diz que *value semantics* deve ser utilizado para reference types.

 A ideia geral aqui é que a escolha do data seemantics utilizada em `range` não tem por base o que a estrutura, mas sim o que ela faz. Portando no `example4.go` temos um exemplo da forma errada do uso de `range`.

 ### Type Assertion

 Análise de `./interfaces/example4/example7.go`.
 `Println` não precisa de decisão de uso quando ao data semantics porque a sua implemntação aceita qualquer type da linguagem. Para fazer algo semelhante ao que `Println` faz é possível utilizar o método da função `myPrintln`, através de interface vazia e *type assertion* que resulta no uso de polimorfismo.