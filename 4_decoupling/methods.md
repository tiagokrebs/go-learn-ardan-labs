## Methods

### Value & Pointer Semantics
Exemplo de reference types da linguagem (`./methods/example5/example5.go`):
```
type IP []byte
type IPMask []byte
```
O type base nesse caso é o slice (value semantics). Ao contruiur uma API utlizando um type é importante identificar e utilizar o data semantic implícito (value ou pointer) para manter a consistência do código e evitar bugs.
No exemplo acima os types utilizam *value semantics*, logo, seria contra esse guideline utilizar ponteiros com os mesmos. 

As exceções para essa regra são no uso de *decode* ou *marshal*. Nessas exceções é aceitável fazer a transição de *value* para *pointer* semantics, porém, o contrário não se aplica. Não é indicado alterar a semântica de *pointer* para *value*.

Exemplo de reference type struct da linguagem (`./methods/example5/example5.go`):
```
type Time struct {
	sec  int64
	nsec int32
	loc  *Location
}
```

Uma forma de pensar sobre a utilização de *pointer semantics* é se ao modificar a informação você stá modificando ela por completo ou apenas parte dela além da característica de unicidade da mesma. Considerando um `type struct person {...}` ao modificar apenas o atributo `name string` nós estamos modificando apenas parte do dado e `person` é único, logo, *pointer semantics* é o mais indicado.

Quando houver dúvida sobre qual data semantics utilizar, mesmo sendo mais propenço a criação de bugs, opte por *pointer*.

Uma forma de verificar a semântica do type é verificar no pacakge do type: o próprio *type*, as *factory functions* e os *methods*.

**Guideline para a estrutura de um package**  
Escrevemos os packages na seguinte ordem:
* type
* factory functions
* methods

Não separe os métodos pelo code base em múltiplos arquivos, mantenha os três juntos.
Não é necessário separar um arquivo para cara type, considere melhorar a *readability*.

**Builtin, Reference e Struct types**
* Builtin: sempre value semantics
* Reference: value semantics em geral com poucas exceções em casos de uso de decode e marshal
* Struct: varia entre value e pointer, é necessário decidir de acordo com os types, factory e methods envolvidos

### Function/Method Variables
Quando sabemos o data semantics sabemos o comportamento, quando sabemos o comportamento sabemos o custo, logo, temos a base para a engenharia.

