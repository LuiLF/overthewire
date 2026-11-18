# Bandit — Nível 0 → 1

## Objetivo

Localizar e ler o arquivo que contém a senha necessária para acessar o próximo nível do Bandit.

## Procedimento

Após estabelecer a conexão SSH com o usuário `bandit0`, utilizei `ls` para listar os arquivos presentes no diretório atual.

A listagem revelou um arquivo chamado `readme`. Para visualizar seu conteúdo, utilizei `cat`:

```bash
ls
cat readme
```

## Resultado

A senha do usuário `bandit1` foi localizada no arquivo `readme`, concluindo o desafio.

---

[← Nível anterior](./level-00.md) · [Voltar ao índice](../README.md)
