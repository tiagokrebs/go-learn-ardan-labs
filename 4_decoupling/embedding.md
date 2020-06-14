## Embedding

Embedding é utilizado em situações onde é necessário reuso de types.

Análise de `./embedding/example1/example1.go`.
Esse exemplo não utiliza embedding, `person` na struct `admin`está sendo utilizado apenas como um campo baseado em `user`. 

Análise de `./embedding/example2/example2.go`.
Nesse exemplo ao retirar o campo `person` da struct `admin` e utilizar o type `user` de forma explícita estamos aplicando embedding.

Nesse caso podemos dizer que *outher type* `admin` possui um *inner type* `user`, o que nos abre a possibilidade da promoção de todos os campos e funções do *inner type* para o *outher type*. O conceito aqui não é de herança (subtype), `admin` continua sendo `admin` e não pode ser utilziado como `user` em nenhum momento, o mesmo vale pra `user`. Embedding se trata apenas dos campos e funções equanto os types de cada estrutura permanecem consistentes.

