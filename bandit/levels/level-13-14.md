# Bandit — Nível 13 → 14

## Objetivo

Acessar o usuário `bandit14` utilizando uma chave privada SSH disponibilizada no diretório inicial do `bandit13`. A senha do próximo nível está armazenada em `/etc/bandit_pass/bandit14` e só pode ser lida pelo usuário correspondente.

## Exploração

Identifiquei o arquivo `sshkey.private` no diretório inicial e tentei utilizá-lo para autenticação SSH.

A primeira tentativa, utilizando apenas `ssh -i sshkey.private`, retornou a sintaxe do comando. Percebi que, além da chave, era necessário informar o usuário e o servidor de destino.

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

O servidor recusou a conexão porque o OverTheWire bloqueia conexões SSH entre níveis realizadas via localhost. Consultei o arquivo de dicas do desafio, que confirmou essa restrição e orientou a realizar a conexão diretamente pelo computador local.

## Resolução

Encerrei a sessão do `bandit13` e utilizei o PowerShell do Windows para transferir a chave privada por SCP:

```powershell
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private Downloads
```

Durante a tentativa inicial, utilizei `-p` em vez de `-P` e não indiquei corretamente a origem remota do arquivo. Após corrigir a sintaxe e autenticar como `bandit13`, a transferência foi concluída.

Com a chave armazenada localmente, estabeleci uma conexão SSH utilizando o arquivo como identidade:

```powershell
ssh -i Downloads/sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

A autenticação foi realizada com sucesso, permitindo acessar a sessão do usuário `bandit14`.

## Observações

- O parâmetro `-i` do SSH especifica o arquivo de chave privada utilizado na autenticação.
- No SCP, `-P` maiúsculo define a porta da conexão; `-p` possui outra finalidade.
- O formato `usuario@servidor:caminho` identifica um arquivo remoto no SCP.
- A origem de uma conexão SSH pode influenciar sua aceitação pelo servidor.
- As mensagens de erro foram essenciais para identificar a restrição de localhost e corrigir os comandos.
- Uma chave privada SSH permite autenticação sem precisar transmitir a chave ao servidor. Ela deve ser protegida e não deve ser publicada em repositórios.

---

[← Nível anterior](level-12-13.md) · [Voltar ao índice](../README.md)
