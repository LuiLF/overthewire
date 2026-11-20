# Bandit — Nível 17 → 18

## Objetivo

Encontrar a senha do próximo nível comparando os arquivos `passwords.old` e `passwords.new`, sabendo que a senha está na única linha alterada do arquivo `passwords.new`.

## Exploração

Inicialmente, listei os arquivos disponíveis no diretório inicial:

```bash
ls
```

Resultado:

```text
passwords.new  passwords.old
```

Em seguida, utilizei o comando `diff` com a opção `-q`:

```bash
diff -q passwords.new passwords.old
```

O comando retornou:

```text
Files passwords.new and passwords.old differ
```

Percebi que essa opção apenas informa se os arquivos são diferentes, sem apresentar as alterações.

Após consultar a documentação, utilizei:

```bash
diff --suppress-common-lines passwords.new passwords.old
```

O resultado indicou uma única linha alterada:

```diff
42c42
< [conteúdo alterado em passwords.new]
---
> [conteúdo original em passwords.old]
```

## Resolução

A saída do `diff` indicou que a linha 42 havia sido modificada.

Como `passwords.new` foi informado como primeiro argumento, a linha marcada com `<` correspondia ao conteúdo desse arquivo.

Utilizei essa linha como senha do próximo nível.

## Observações

- `diff` compara arquivos e, por padrão, exibe suas diferenças.
- A opção `-q` informa apenas se existem diferenças, sem detalhá-las.
- No formato padrão, `<` identifica o conteúdo do primeiro arquivo e `>` o conteúdo do segundo.
- A indicação `42c42` significa que a linha 42 do primeiro arquivo foi alterada em relação à linha 42 do segundo.
- A opção `--suppress-common-lines` não era necessária nesse caso, pois o formato padrão do `diff` já omite as linhas iguais.

---

[← Nível anterior](level-16-17.md) · [Voltar ao índice](../README.md)
