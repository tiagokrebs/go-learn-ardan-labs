## Maps
Seguindo regras de consistência maps, assim como slices, não devem ser criados de forma a instanciar *zero values* (`./maps/example1/example1.go`).

Para checar a presenã em um map é possível utilizar
```
score, ok := scores["anna"]
```
Um elemento não existente no *map* vai receber um *zero value*. A variável `ok` no exemplo acima recebe um identificador *boolean* que representa a existência do elemento no map 
enquanto `score` representa o valor (`./maps/example2/example2.go`).

Apenas elemntos capazes de participar de uma operação condicional podem ser utilizados como *key* de um *map*. Um `if` ou um *slice* não pode ser uma *key* (`./maps/example3/example3.go`).

*range* sobre *map* segue ordem aleatória builtin (`./maps/example4/example4.go`).

É possível ordenar *map* através das funções do *sort* package (`./maps/example5/example5.go`).

*map* são *referece types*, logo usam ponteiros (pointer semantics) (`./maps/example6/[example6.go|example7.go]`). Em arquiteturas multithread é importante dar atenção a pontos de utilização do *map* por causa dessa sua característica.