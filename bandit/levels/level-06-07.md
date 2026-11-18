# Bandit — Nível 6 → 7

## Objetivo

Encontrar a senha do próximo nível, armazenada em algum lugar do servidor, em um arquivo com as seguintes características:

- Pertencente ao usuário `bandit7`.
- Pertencente ao grupo `bandit6`.
- Tamanho de exatamente 33 bytes.

## Exploração

Inicialmente, identifiquei que poderia utilizar o comando `find` para combinar os três critérios de busca: proprietário, grupo e tamanho.

Após consultar a sintaxe das opções `-user` e `-group`, executei:

```bash
find -size 33c -user bandit7 -group bandit6
```

A busca não retornou resultados porque estava sendo executada a partir do diretório pessoal do usuário `bandit6`.

Como o enunciado indicava que o arquivo poderia estar em qualquer lugar do servidor, passei a realizar a busca a partir da raiz do sistema de arquivos (`/`).

Durante a execução, apareceram diversas mensagens de `Permission denied`, referentes a diretórios que o usuário não tinha permissão para acessar.

## Resolução

Para facilitar a identificação dos resultados, aprendi a utilizar o redirecionamento `2>/dev/null`, que descarta as mensagens enviadas para a saída de erro (`stderr`).

A partir do diretório `/`, executei:

```bash
find -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

O comando retornou:

```text
./var/lib/dpkg/info/bandit7.password
```

Em seguida, utilizei `cat` para ler o conteúdo do arquivo e obter a senha do próximo nível.

## Observações

- O `find` permite combinar critérios de busca, como proprietário (`-user`), grupo (`-group`) e tamanho (`-size`).
- Os critérios são combinados por um AND implícito, sem necessidade de utilizar um operador entre eles.
- Quando nenhum caminho é informado, o `find` inicia a busca no diretório atual.
- O redirecionamento `2>/dev/null` descarta mensagens de erro, mas não altera os critérios da busca.
- Erros de permissão não significam necessariamente que a busca inteira falhou.

---

[← Nível anterior](level-05-06.md) · [Voltar ao índice](../README.md)
