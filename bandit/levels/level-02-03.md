# Bandit — Nível 2 → 3

## Objetivo

Encontrar a senha do próximo nível em um arquivo chamado `--spaces in this filename--`, localizado no diretório pessoal do usuário `bandit2`.

## Exploração

Após identificar o arquivo com `ls`, tentei ler seu conteúdo utilizando aspas para preservar os espaços no nome:

```bash
cat "--spaces in this filename--"
```

Entretanto, o comando retornou um erro porque o nome começava com `--`, fazendo com que o `cat` o interpretasse como uma opção.

## Solução

Para evitar essa interpretação, utilizei `--` antes do nome do arquivo:

```bash
cat -- "--spaces in this filename--"
```

O primeiro `--` encerra a interpretação de opções, enquanto as aspas mantêm o nome completo como um único argumento.

Dessa forma, consegui acessar o conteúdo e recuperar a senha do próximo nível.

---

[← Nível anterior](./level-01-02.md) · [Voltar ao índice](../README.md)
