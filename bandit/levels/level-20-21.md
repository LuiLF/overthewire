# Bandit — Nível 20 → 21

## Objetivo

Utilizar um executável com permissão SetUID chamado `suconnect`, que estabelece uma conexão TCP com uma porta informada pelo usuário.

O programa recebe uma linha de texto pela conexão e a compara com a senha do nível 20. Caso a senha esteja correta, transmite a senha do nível 21 pela mesma conexão.

## Exploração

Inicialmente, executei o `suconnect` com argumentos diferentes para entender seu funcionamento:

```bash
./suconnect ls
./suconnect
./suconnect 1
```

O programa apresentou a seguinte instrução:

```text
Usage: ./suconnect <portnumber>
```

Com isso, confirmei que ele esperava um número de porta como argumento.

### Investigação das portas TCP

Minha primeira abordagem foi utilizar o Nmap para identificar serviços disponíveis no `localhost`:

```bash
nmap localhost
```

Encontrei diversas portas abertas e tentei estabelecer conexões com elas utilizando o `suconnect`.

Por exemplo:

```bash
./suconnect 22
```

O programa recebeu a identificação do servidor SSH, mas rejeitou o conteúdo por não corresponder à senha esperada.

Também testei outras portas e tentei digitar a senha diretamente no terminal do `suconnect`, sem sucesso.

Essa investigação ajudou a identificar uma diferença importante: o `suconnect` não lê a senha diretamente do teclado. Ele espera recebê-la pela conexão TCP.

### Estudo do tmux e gerenciamento de processos

O enunciado sugeria ferramentas como `screen`, `tmux` e recursos de controle de tarefas do shell.

Pesquisei o funcionamento do `tmux` e aprendi a criar sessões persistentes, alternar entre terminais e retornar à sessão original sem encerrar os processos.

Também utilizei `Ctrl+Z` durante os testes, descobrindo que esse atalho suspende o processo, diferentemente de encerrá-lo.

### Investigação do Netcat

Depois de compreender que precisava de um programa escutando uma conexão TCP, passei a investigar o `nc`.

Consultei sua documentação e identifiquei a opção `-l`, utilizada para aguardar conexões.

Minha primeira tentativa foi:

```bash
nc -l localhost 777
```

O programa retornou:

```text
nc: Permission denied
```

A porta escolhida estava abaixo de 1024, faixa normalmente reservada a processos com privilégios específicos no Linux.

Após escolher uma porta mais alta, o Netcat permaneceu em execução sem apresentar mensagens.

Inicialmente, interpretei esse silêncio como um possível problema, mas compreendi que um programa em modo de escuta pode simplesmente permanecer aguardando uma conexão.

Também tentei utilizar SSH para acessar a porta escolhida, antes de perceber que o SSH utiliza seu próprio protocolo e não era necessário para essa comunicação.

## Resolução

Utilizei dois ambientes de terminal na mesma máquina, mantendo o Netcat em execução dentro de uma sessão do `tmux`.

No terminal com o Netcat, iniciei a escuta na porta escolhida:

```bash
nc -l 55555
```

No outro terminal, executei:

```bash
./suconnect 55555
```

Inicialmente, interpretei o silêncio do Netcat como um possível problema, pois ele não apresentava nenhum prompt solicitando a senha.

Mesmo após conectar o suconnect, não sabia onde inserir a senha. O ponto decisivo foi perceber que o próprio terminal do nc, embora aparentemente parado, continuava aceitando entrada do teclado.

Digitei a senha atual diretamente no terminal do Netcat e pressionei Enter. O suconnect recebeu a senha pela conexão TCP e confirmou a validação:

```text
Read: [senha atual omitida]
Password matches, sending next password
```

No terminal do Netcat, apareceu a senha do nível seguinte:

```text
[senha atual omitida]
[senha do próximo nível omitida]
```

**Nível concluído.**

## Observações

- **Cliente e servidor TCP:** o `suconnect` inicia a conexão, enquanto o `nc -l` aguarda uma conexão na porta escolhida.
- **Comunicação bidirecional:** a mesma conexão TCP permite enviar a senha ao `suconnect` e receber a resposta.
- **Entrada padrão:** um programa de terminal pode aceitar texto digitado mesmo sem apresentar um prompt.
- **Portas privilegiadas:** portas abaixo de 1024 normalmente exigem privilégios específicos para escuta.
- **tmux:** permite manter um processo em execução enquanto trabalho em outro ambiente de terminal.
- **Controle de tarefas:** `Ctrl+Z` suspende um processo; isso não equivale a mantê-lo executando em segundo plano.
- **ELF e SetUID:** ELF identifica o formato do executável, enquanto SetUID está relacionado às permissões com que ele é executado.

### Aprendizado

Este nível exigiu compreender não apenas a sintaxe dos comandos, mas também o fluxo de comunicação entre dois processos.

A maior dificuldade foi identificar de qual lado da conexão a senha deveria ser enviada e perceber que o terminal do Netcat podia receber entrada diretamente.

A resolução contou com consultas à documentação e orientação durante a investigação. Pretendo refazer o desafio posteriormente, sem assistência, para verificar se consigo reproduzir e explicar o processo de forma independente.

---

[← Nível anterior](level-19-20.md) · [Voltar ao índice](../README.md)
