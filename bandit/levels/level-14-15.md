# Bandit — Nível 14 → 15

## Objetivo

Obter a senha do próximo nível enviando a senha atual para um serviço disponível na porta `30000` do `localhost`.

## Exploração

Analisei os comandos sugeridos pelo desafio e identifiquei o Netcat (`nc`) como uma ferramenta adequada para estabelecer uma conexão TCP e enviar dados ao serviço.

Inicialmente, tentei realizar a conexão utilizando:

```bash
nc 127.0.0.1 3000
```

A conexão não foi estabelecida porque utilizei a porta `3000` em vez de `30000`.

Também tentei utilizar o parâmetro `-p`, mas a mensagem de uso do Netcat indicou que essa opção define a porta de origem (*source port*), e não a porta de destino.

## Resolução

Após identificar o erro, executei o comando com a porta correta:

```bash
nc 127.0.0.1 30000
```

Com a conexão estabelecida, enviei a senha do `bandit14` diretamente pelo terminal.

O serviço respondeu com `Correct!` e retornou a senha do próximo nível, concluindo o desafio.

## Observações

- `127.0.0.1` é um endereço IPv4 de loopback, utilizado para comunicação com a própria máquina.
- `localhost` também poderia ser utilizado como destino, dependendo da resolução de nomes e da configuração do serviço.
- O Netcat permite estabelecer conexões TCP e UDP para enviar e receber dados.
- No `nc`, a porta de destino pode ser informada diretamente após o endereço, sem utilizar `-p`.
- Conferir os argumentos e as mensagens de erro ajuda a identificar problemas simples antes de investigar causas mais complexas.

---

[← Nível anterior](level-13-14.md) · [Voltar ao índice](../README.md)
