# Bandit — Nível 13 → 14

## Objetivo

Acessar o usuário `bandit14` utilizando uma chave privada SSH disponibilizada no diretório inicial do `bandit13` e obter a senha armazenada em `/etc/bandit_pass/bandit14`.

## Exploração

Encontrei o arquivo `sshkey.private` e tentei utilizá-lo para autenticação SSH.

Inicialmente, executei `ssh -i sshkey.private` sem informar o destino, recebendo a mensagem de uso do comando. Após corrigir a sintaxe, tentei conectar diretamente ao `bandit14`:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

A conexão foi recusada porque o OverTheWire bloqueia conexões SSH entre níveis realizadas via localhost.

Ao consultar o arquivo de dicas disponível no diretório inicial, confirmei essa restrição e identifiquei a necessidade de realizar a conexão a partir do meu computador.

## Resolução

Encerrei a sessão SSH do `bandit13` e utilizei o PowerShell do Windows para transferir a chave privada:

```powershell
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private Downloads
```

Na primeira tentativa, utilizei `-p` em vez de `-P` e não especifiquei corretamente o caminho remoto. Após corrigir a sintaxe e autenticar como `bandit13`, a transferência foi concluída.

Com a chave disponível localmente, estabeleci uma conexão SSH como `bandit14`:

```powershell
ssh -i Downloads/sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

A autenticação foi realizada com sucesso. Já na sessão do `bandit14`, consultei o arquivo indicado no enunciado:

```bash
cat /etc/bandit_pass/bandit14
```

O comando retornou a senha do usuário, concluindo o objetivo do nível.

## Observações

- O parâmetro `-i` do SSH especifica a chave privada utilizada na autenticação.
- No SCP, `-P` maiúsculo define a porta da conexão, diferentemente do SSH, que utiliza `-p`.
- A sintaxe `usuario@servidor:caminho` permite identificar arquivos remotos durante transferências com SCP.
- A chave privada e a senha são credenciais diferentes que podem permitir autenticação na mesma conta.
- A leitura das mensagens de erro foi essencial para identificar a restrição de localhost e corrigir a abordagem.
- Chaves privadas devem ser protegidas e não devem ser publicadas em repositórios.

---

[← Nível anterior](level-12-13.md) · [Voltar ao índice](../README.md)
