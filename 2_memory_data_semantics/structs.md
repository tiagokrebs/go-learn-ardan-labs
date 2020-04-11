## Structs

"Implicit conversion of types is the Halloween special coding. Whoever thought of them deserves their own special hell." ~ Martin Thompson

 ```
 type example struct {
     flag       bool # 1 byte
     counter    int16 # 2 bytes
     pi         float32 # 4 bytes 
 }
 ```

 Para a struct o compilador vai alocar um espaço contínuo de memória para os types utilizados mais 1 byte de alinhamento (padding byte), no exemplo acima 8 bytes.
 Blocos de 8 bits são alocados de acordo com o alinhamento do tamanho dos inteiros armazenados.

**Readability é mais importante que micro otimização de padding bytes.**