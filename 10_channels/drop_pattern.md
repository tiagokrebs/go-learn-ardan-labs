### Drop pattern

Análise de `./channels/example1`. O drop pattern é sobre entender a capacidade da aplicação, tudo o que não pode ser atendido imediatamente ou ser colocado em estado pendende de realização é descartado. Nesse exemplo a capacidade é de 100 requisições, esse valor define o tamamho do buffer na linha 223.
Na linha 226 uma goroutine é disparada e na linha 226 é bloqueada aguardando por instruções serem enviadas ao channel. Aqui não está sendo utilizado o método de pooling, o que é totalmente possível com o drop pattern.
Na linha 232 inicia-se o envio de 2000 mensagens para o channel com capacidade 100, que deve detectar a tentativa de ultrapassar esse limite e fazer drop dos requets excedentes. Essa ação é feita com o `select case statement` (linha 233), que permite que a mesma goroutine realize send e receive para o mesmo channel.

na linha 234 a goroutine tenta enviar um signal para o channel, que por sua ves pode estar bloqueado devido ao seu buffer de 100 ou não. Nesse caso a cláusula `defaul` é o drop.