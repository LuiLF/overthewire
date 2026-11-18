# Bandit — Nível 0

## Objetivo

Estabelecer uma conexão SSH com o servidor do OverTheWire utilizando as informações de acesso disponibilizadas na página oficial do Bandit.

## Procedimento

Utilizei o PowerShell do Windows 10 para estabelecer uma conexão com o servidor remoto.

O comando executado foi:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Após informar a senha fornecida pela plataforma, a autenticação foi concluída e obtive acesso ao terminal Linux do Bandit.

## Comandos utilizados

| Comando | Finalidade |
|---|---|
| `ssh` | Estabelecer uma conexão remota segura |
| `-p` | Especificar a porta utilizada na conexão |

## Resultado

Conexão SSH estabelecida com sucesso, permitindo o acesso ao ambiente remoto do Bandit.

## Aprendizado

Pratiquei a utilização do SSH para acessar um sistema Linux remoto, identificando os elementos necessários para a conexão: usuário, endereço do servidor, porta e senha.

---

[← Voltar ao Bandit](../README.md)
