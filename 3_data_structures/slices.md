## Slices

Slice pode ser considerado um vetor ou um array dinâmico.

### Declare, Length & Reference Types
Slices possuem um tamanho e uma capacidade associados. Por default a capacidade é o tamanho do slice (`./slices/example1/eexample1.go`) porém a capacidade pode ser definida previamente (`./slices/example2/example2.go`). Para efeitos de length o tamanho sempre é considerado, ou seja, na iteração em `example2.go` apesar da capacidade 8 apenas 5 elementos serão exibidos.
Utilizar *make* para acriação de slices é indicado para melhor readability.

### Appending Slices
Em `./slices/example4/example4.go` vemos a função *append* que também trabalha sobre *value semantics* (com a sua própria cópia do slice). No final do *append* a informação resultante é copiada para a informação original o que define essa operação como *value semantics mutation*.

### Taking Slices of Slices
Slice de um slice é representado por `[a, a + N posições]` e utiliza *pointer semantics*, o que pode criar alguns efeitos não esperado como vistos em `./slices/example3/example3.go` quando alteramos uma posição do slice de outro slice.

### Slices & References
`./slices/example5/example5`

### Strings & Slices
`./slices/example6/example6`

### Range Mechanics
Em `./slices/example8/example8` na primeira iteração com slice é possível a utilização da cópia do valor (value semantics) enquanto na segunda a utilização de pointeiros (pointer semantics) builtin da instrução *range* que efetua mutação no slice durante a iteração.