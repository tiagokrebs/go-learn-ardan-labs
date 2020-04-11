## Pointers

### Pass by values
Informação passada para diferentes partes do programa/fronteiras através de cópia.
Uma função em golang recebe um frame da memória (mínimo 2k) para cada função, quando uma fronteira é ultrapassada o programa necessita de um novo frame no stack.

Stack       | Memória | 
---         | --- |
func main() | count (8 bytes) |
increment(count int) | count (8 bytes) |

*main* e *increment* tem o seu *frame* de memória no stack que atua como um sandbox, cada função só tem acesso a informação dentro do seu frame.

Como cada função acessa apenas o conteúdo do seu próprio frame, caso seja necessário trocar informação entre funções ela é copiada.

```
increment(count int) // count é a cópia total da informação
```

Value semantics: toda função recebe a sua cópia da informação trocada. Esse é um método seguro e que pode ter melhor alinhamento com o modelo de hardware e multi thread, porém, também pode inserir custo na eficiência fazendo com que não seja uma escolha viável. 

### Sharing data
Com a utilização de ponteiros é possível compartilhar o endereço ou valor da informação conforme desejado

Stack       | Memória | 
---         | --- |
func main() | count (8 bytes) |
increment(count int) | *count (1 bytes) |

```
increment(count *int) // count é a cópia do endereço
```

**int* indica que *increment* recebe apenas o valor da informação. Dessa forma não é necessário que uma cópia do dado seja realizada para o frame de memória da função.

Pointer semantics: compartilhamento da informação (acesso indireto a memória).

### Escape analysis
Static code analysis é um dos métodos de escape analysis responsável por passar ao compilador a decisão de onde um valor deve ser contruído na memória, no stack ou na heap. Quando a decisão é a heap chamamos isso de scape.

```
return &u
```

O comando acima é um caso onde estamos retornando um endereço de memória para o stack acima. Em outros compiladores como C isso não é possível porque o stack é limpo no final da função fazendo com que o stack perca o endereço da informação (bug).
Em o Go o stack também é limpo porém o compilador decide guardar esse endereço de memória na heap e faz a recuperação para o stack superior quando necessário.

Quando a heap é utilizada a reponsabilidade de limpeza da memória para a ser do garbage collector enquando o stack é automaticamente limpo quando uma função chega ao fim.

Para análise de scapes no compilador do GO.
```
go build -gcflags -m2

./example4.go:46:9: &u escapes to heap
./example4.go:46:9: 	from ~r0 (return) at ./example4.go:46:2
./example4.go:39:2: moved to heap: u
```

### Stack growth
Em Go o stack começa com tamanho de 2Kb, quando um stack maior é necessário o stack é aumentado (4Kb) e toda informação é movida para o novo stack (stack contínuo).
O custo dessa operação é de ns (nano).

### Garbage collection
O trabalho da GC é procurar na memória heap espaços alocados na memória que não estão em uso para fazer a limpeza, permitindo que ele seja utilizado novamente. Go usa CMS collector.