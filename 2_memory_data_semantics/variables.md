## Variables
Fundamento de custo em types, código escrito e memória.

Golang é sobre types

### Built-in
Memória pode ser representada por blocos de 8 bits.
00001010 -> 1 byte

Types em golang podem ser utilizados para otimização do uso de memória (baseada em integer).
```
var a float64 # 64 bits = 8 bytes
var b bool # 8 bits = 1 byte
var c int8
var d int16
var e int32
var f int64
```

O integer mais eficiente varia de acordo com a arquitetura utilizada.
* 32x -> 32 bits (4 bytes) é mais eficiente;
* 64x -> 64 bits (8 bytes) é mais eficiente;

### Zero value
Toda variável precisa ser inicializada, se o código não o fizer o compilador definirá o zero value de acordo com o tipo e a arquitetura.
```
var a int # inicializada com 0
var b string # inicializada com ""
```

Short variable declaration é utilizado na inicialização de variáveis.
```
aa := 10
bb := "hey"
```

### Conversion (not casting)
Cast implica em alocar memória suficiente para que a operação seja efetuada (non optimal).
Conversão aloca o espaço correto em memória para o type e tranfere a informação (mais integro).
```
aaa := int32(10)
```