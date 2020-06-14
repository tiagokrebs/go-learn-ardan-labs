## Grouping Types

A good API is not just easy to use but also hard to misuse ~ JBD (Google)
You can allways embed, but you cannot deompose big interfaces once they are out there. Keep interfaces small ~ JBD
Don't design with interfaces, discover them ~ Rob Pike
Dupliation is far cheaper than the wrong abstraction ~ Sandi Metz


`./grouping/example1/example1.go` é um exmplo de design a não ser utilizado. A ideia de que as structs `Dog` e `Cat` podem ser adicionadas a uma coleção da struct `Animals` está baseada no uso de sub types, esse não é o coneito aplicado em Go.
Na sessão anterior (decoupling) foi mostrado que interfaces devem ser utilizadas como fontes geradoras de polimorfismo para que strucs sejam agrupadas pelo o que elas fazem e não pelo que elas são.

Em `./grouping/example1/example1.go` o uso de interfaces foi utilizado para a função `Speak` enquanto a generalização da struct `Animal` foi removida. Quando uma coleção das structs `Dog` e `Cat` precisa ser criada isso é feito com base em que ambos "podem falar" e não porque "são animais" como no exeplo aterior, ou seja, ambos são agrupados pela interface `Speaker` através de função `Speak()`.