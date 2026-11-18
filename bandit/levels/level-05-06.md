# Bandit — Nível 5 → 6

## Objetivo

Localizar a senha do próximo nível em um arquivo dentro do diretório `inhere`, com as seguintes características:

- Legível por humanos (*human-readable*).
- Tamanho de exatamente 1033 bytes.
- Não executável.

## Exploração

Ao acessar `inhere`, encontrei 20 subdiretórios, cada um contendo diversos arquivos.

Inicialmente, utilizei `find` para explorar a estrutura de diretórios e identificar os arquivos disponíveis:

```bash
find maybehere*
```

Também consultei o espaço ocupado pelos diretórios com `du`:

```bash
du maybehere*
```

Como o desafio informava o tamanho exato do arquivo, concentrei a busca nesse critério.

## Resolução

Utilizei a opção `-size` do `find` com o sufixo `c`, que permite especificar o tamanho em bytes:

```bash
find .. -size 1033c
```

O comando retornou um único resultado:

```text
../inhere/maybehere07/.file2
```

Em seguida, acessei o diretório correspondente e li o arquivo:

```bash
cd maybehere07
cat ./.file2
```

O conteúdo permitiu recuperar a senha necessária para avançar.

## Observações

A busca pelo tamanho exato foi suficiente para localizar o arquivo neste desafio, sem necessidade de combinar os demais critérios do enunciado.

O exercício também permitiu diferenciar duas ferramentas:

- **`find`:** localiza arquivos com base em critérios, como tamanho, tipo e permissões.
- **`du`:** informa o espaço ocupado em disco, que pode ser diferente do tamanho lógico do arquivo.

---

[← Nível anterior](./level-04-05.md) · [Voltar ao índice](../README.md)
