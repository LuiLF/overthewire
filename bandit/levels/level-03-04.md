# Bandit — Nível 3 → 4

## Objetivo

Localizar a senha do próximo nível em um arquivo oculto dentro do diretório `inhere`.

## Resolução

O enunciado já indicava que o arquivo estava oculto. Após identificar o diretório `inhere`, acessei seu conteúdo e listei os arquivos, incluindo os ocultos:

```bash
ls
cd inhere/
ls -a
```

A listagem revelou o arquivo `...Hiding-From-You`, que foi lido com:

```bash
cat -- "...Hiding-From-You"
```

O conteúdo exibido permitiu recuperar a senha do próximo nível.

---

[← Nível anterior](./level-02-03.md) · [Voltar ao índice](../README.md)
