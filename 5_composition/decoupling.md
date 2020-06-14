## Decoupling

Nessa aula é dado um exemplo de construção de uma API em camadas que segue a seguinte ordem: primitiva, baixo nível e alto nível, seguindo o modelo de decoupling e uso de interfaces para uso de polimorfismo.
O conceito de contrução de uma API em camadas faz relação com a sessão de testes.

Análise de `./decoupling/example1/example1.md`, criação da API.

API primitiva: 
* Structs `Data`, `Xenia`, `Pillar` e `System` definem como a API é estruturada
* Funções `Pull` e `Store` definem primeiro exemplo de obtenção/armazenamento do dado
* Nessa etapa não é levado em consideração itens de performance, apenas eestrutura inicial para solução do problema proposto.

API baixo nível: 
* Funções `pull` e `store` implementam `Pull` e `Store` em formado de bulk actions

API de alto nível:
* Função `Copy` faz *pull* e *store* em batches até que não haja mais dados

Análise de `./decoupling/example2|3|4/example2|3|4.md`, decoupling.
Nessas etapas as interfaces `Puller` e `Storer` são criadas para que `System` não seja o agrupador de sub types de `Xenia` e `Pillar` mas sim de forma que sejam agrupados pelas ações que ambos tomam: `Pull` e `Store`. 
Também é criada a interface `PullStorer` composta das interfaces `Puller` e `Storer` para uso na função `Copy` na high level API. A ideia principal desse exemplo é permitir que o comportamento da API possa ser utilizada por outros sistemas e não apenas Xenia e Pillar.
Por último o type `System` deixa de ser `Xenia` e `Pillar` e passa para `Puller` e `Storer`, dessa forma basta fazer a construção dos diferentes sistemas em `main` para que a API possa fazer o seu trabalho.

Em `./decoupling/example5|6/example5|6.md` readability do código é melhorada. You are allways drafting code.