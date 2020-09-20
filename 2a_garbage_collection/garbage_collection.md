## Garbage Collection

Seção adicionada pós lançamento do curso.

O GC em Go é considerado:
- Não geracional
- Não compactado
- Concorrente
- Tri-color marking ou mark-and-sweep (algoritmos base)

Talvez o aspecto mais importante seja o fato do GC ser concorrente, o que está alocado na heap não é movido. Uma vez que a alocação é feita na heap aquele endereço está fixado até o final do lifetime daquela alocação.

o GC em Go possui duas prioridades principais: melhorar o throughput da aplicação durante a coleta e uso de recurso da memória.

É possível administrar de certa forma o início da coleta do GC através da variável de ambiente **percentual de GC** descrita no package debug: `$GOGC` (defaul 100%). Utilizando o valor default a prinmeira coleta vai acontecer quando a heap atingir 4MB de uso. As coletas posteriores sempre seão 100% do `marked alive` registrado na última coleta (mais detalhes no artigo abaixo).

O GC é executado em três diferentes fases:
- Mark start: essa etapa é bloqueante, a aplicação não está em execução nesse momento;
- Marking: Marca os valores de memória, essa etapa é concorrente portanto ocorre ao mesmo tempo de execução da aplicação;
- Mark termination: segunda etapa bloqueante;

Um dos objetivos para o GC não adicionar latência a aplicação é garantir que as etapas bloqueantes sejam sempre pequenas e consistentes. O tempo total dessas duas etapas somadas deve idealmente ser menor quwe 100us (micro segundos).

Durante a etapa de Marking a(s) goroutine(s) do GC pela sua característica concorrente não bloquearão a execução da aplicação, porém elas **tomarão 25% da capacidade da CPU para si** deixando a aplicação abaixo da sua capacidade total de throughput. Porém ainda é possível o GC parar a apliacação em outras threads para o trabalho de `mark assist` tomando 50% da capacidade total, isso acontece quando o GC não atinge seu objetivo de desempenho (mais detalhes no artigo abaixo).
É importante levar esse comportamento em consideração especialmente para aplicações em nuvem, onde frações de CPU são prática comum. Em um exemplo simples com CPU de 4 cores é possível dizer que o GC vai se comportar como uma aplicação single thread.

[Exemplo completo + GC Trace](https://www.ardanlabs.com/blog/2018/12/garbage-collection-in-go-part1-semantics.html).

