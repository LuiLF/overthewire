# Bandit — Nível 9 → 10

## Objetivo

Encontrar a senha do próximo nível no arquivo `data.txt`, armazenada em uma das poucas sequências de texto legíveis e precedida por vários caracteres `=`.

## Exploração

Inicialmente, tentei utilizar `grep` para localizar os caracteres `=` no arquivo. Entretanto, o comando retornou:

```text
grep: data.txt: binary file matches
```

Isso ocorreu porque o arquivo continha dados binários, impedindo a visualização convencional das linhas correspondentes pelo `grep`.

Também experimentei utilizar `--` e `*`, mas essas tentativas não resolveram o problema.

Ao reler o enunciado, identifiquei o comando `strings`, utilizado para extrair sequências de caracteres imprimíveis de arquivos.

Executei:

```bash
strings -a data.txt
```

A saída apresentou diversos trechos legíveis, incluindo algumas linhas iniciadas por vários caracteres `=`. Entre elas, identifiquei visualmente a linha correspondente à senha.

## Resolução

O comando utilizado para encontrar a senha foi:

```bash
strings -a data.txt
```

A opção `-a` permite examinar todo o arquivo.

Como a senha estava precedida por vários caracteres `=`, foi possível identificá-la diretamente na saída, sem a necessidade de outros filtros.

Após a resolução, explorei a combinação de `strings` com `grep` utilizando o operador `|`:

```bash
strings -a data.txt | grep "=*"
```

Esse comando retornou todas as linhas extraídas, pois, em expressões regulares, `*` permite zero ou mais ocorrências do elemento anterior.

Em seguida, testei:

```bash
strings -a data.txt | grep "="
```

Dessa vez, foram exibidas apenas as linhas que continham pelo menos um caractere `=`, facilitando a identificação da senha.

## Observações

- O `strings` permite extrair sequências de caracteres imprimíveis de arquivos, inclusive binários.
- A opção `-a` faz com que o comando examine todo o arquivo.
- O `grep` pode identificar correspondências em arquivos binários sem exibir seu conteúdo normalmente.
- O operador `|` encaminha a saída de um comando para a entrada de outro.
- Ao informar um arquivo diretamente ao `grep`, ele realiza a busca nesse arquivo, em vez de consumir a entrada recebida pelo pipe.
- Em expressões regulares, `*` representa zero ou mais ocorrências do elemento anterior, diferentemente do curinga utilizado pelo Bash para nomes de arquivos.

---

[← Nível anterior](level-08-09.md) · [Voltar ao índice](../README.md)
