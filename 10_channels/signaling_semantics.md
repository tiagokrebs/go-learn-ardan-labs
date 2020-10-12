### Signaling semantics

**A goroutine que faz `send` (está fazendo signaling) para o channel precisa de confirmação de que o sinal foi recebida?**
Dependendo da resposta a dinâmica do channel muda, existe uma troca entre a garantia de entrega, a latência da goroutine que espera ou não pela confirmação e a segurança no envio do sinal.
- +Latência: Quando a goroutine espera pela confirmação há segurança/consistência de que o sinal foi recebido, porém latência é adicionada devido a espera do sinal.
- +Risco: Quando a goroutine não espera pela confirmação não há latência no envio do sinal, porém não também há segurança no recebimento. A consistência do recebimento deve ser implementada em outra estrutura auxiliar do sistema.

Um channel não bufferizado garante a confirmação do signal enquanto um channel bufferiado tem o comportamento inverso, não há confirmação.

**Um sinal é esperado com dados ou sem dados?**
- Sinal 1:1. Um sinal é enviado para consumo de apenas uma goroutine.
- Sinal 1:N. Um dinal é enviado para consumo de N goroutines. A prática comum para comunicar com as N goroutines é manipular o estados do channel.

Estados do channel:
- Open: após `make` o channel está pronto para recber escrita/leitura.
- Zero value: envios/recebimentos serão bloqueados, normalmente utilizado para criar rate limits na escrita/leitura do channel.
- Closed: utilizado para cancelamento/shutdown das goroutinas que utilizam o channel.

