## Exporting

Go passou a implementar packaging em um certo ponto. Isso significa que cada pasta no source representa uma unidade única de código (statuic lib).

`./exporting/example1/example1.go` importa o package `counters` que exporta o type `AlertCounter`.
Para que haja exportação por parte do package o label do dado precisa iniciar com letras maiúscula.

Em `./exporting/example2/example2.go` `alertCounter` é eexportado com a primeira letra minúscula, gerando erro na execução uma vez que a exportação do type não foi realizada.