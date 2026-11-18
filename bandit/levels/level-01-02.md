# Bandit — Nível 1 → 2

## Objetivo

Encontrar a senha do próximo nível em um arquivo chamado `-`, localizado no diretório pessoal do usuário `bandit1`.

## Exploração

Após listar o conteúdo do diretório com `ls -a`, encontrei um arquivo chamado `-`.

A princípio, confundi o arquivo com um diretório e tentei acessá-lo usando `cd`. Ao perceber que não era um diretório, experimentei:

```bash
cat -
```

O comando não exibiu o conteúdo esperado, pois o `cat` interpreta o hífen isolado como uma referência à entrada padrão (`stdin`).

## Solução

Para acessar o arquivo, utilizei seu caminho relativo:

```bash
cat ./-
```

O prefixo `./` indica o diretório atual, permitindo que o arquivo seja identificado pelo caminho, sem a interpretação especial do hífen isolado.

Com isso, foi possível recuperar a senha do próximo nível.

## Observação técnica

Nomes de arquivos podem entrar em conflito com a sintaxe dos comandos. Nesses casos, utilizar um caminho explícito ajuda a evitar interpretações inesperadas.

---

[← Nível anterior](./level-00-01.md) · [Voltar ao índice](../README.md)
