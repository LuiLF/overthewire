# Bandit — Nível 0 → 1

## Objetivo

Localizar e ler o arquivo que contém a senha necessária para acessar o próximo nível do Bandit.

## Procedimento

Após estabelecer a conexão SSH com o usuário `bandit0`, utilizei o comando `ls` para listar os arquivos presentes no diretório atual.

A listagem revelou um arquivo chamado `readme`. Como o objetivo era encontrar a senha armazenada nesse arquivo, utilizei `cat` para visualizar seu conteúdo.

```bash
ls
cat readme
```

A leitura do arquivo permitiu identificar a senha necessária para acessar o próximo nível.

## Comandos utilizados

| Comando | Finalidade |
|---|---|
| `ls` | Listar arquivos e diretórios |
| `cat` | Exibir o conteúdo de um arquivo |

## Resultado

A senha do usuário `bandit1` foi localizada no arquivo `readme`, concluindo o desafio.

## Aprendizado

Reforcei o uso de comandos básicos do Linux para identificar arquivos e consultar seu conteúdo diretamente pelo terminal.

O desafio também demonstrou a importância de interpretar o objetivo antes de escolher os comandos necessários, evitando executar ferramentas desnecessárias.

---

[← Voltar ao Bandit](../README.md)
