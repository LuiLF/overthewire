# Bandit — Nível 5 → 6

## Objetivo

Encontrar a senha do próximo nível, armazenada em um arquivo dentro do diretório `inhere`, com as seguintes características:

- Legível por humanos (*human-readable*).
- Tamanho de exatamente 1033 bytes.
- Não executável.

## Exploração

Após acessar `inhere`, encontrei 20 subdiretórios, cada um contendo diversos arquivos com nomes semelhantes.

Inicialmente, utilizei `find` para visualizar os arquivos disponíveis:

```bash
find maybehere*
```

Também explorei o comando `du` para verificar o espaço ocupado pelos diretórios:

```bash
du maybehere*
```

Durante as tentativas, tive dificuldades para aplicar os critérios de busca com `find`, principalmente na sintaxe de `-size` e na unidade utilizada para representar bytes.

## Resolução

Após compreender que a opção `-size` utiliza blocos de 512 bytes por padrão e que o sufixo `c` permite especificar bytes, executei:

```bash
find .. -size 1033c
```

A busca retornou um único arquivo:

```text
../inhere/maybehere07/.file2
```

Em seguida, acessei o diretório correspondente e li seu conteúdo:

```bash
cd maybehere07
cat ./.file2
```

Com isso, encontrei a senha necessária para avançar.

## Observações

Embora o enunciado apresentasse três características, utilizei apenas o tamanho exato na busca, pois ainda não sabia aplicar os demais critérios com `find`.

Após a resolução, revisei alguns conceitos importantes:

- **`find`:** permite combinar critérios para localizar arquivos recursivamente.
- **`-size 1033c`:** procura arquivos com exatamente 1033 bytes.
- **`-type f`:** restringe a busca a arquivos regulares.
- **`-perm`:** permite filtrar arquivos pelas permissões.
- **`du`:** informa o espaço ocupado em disco, que não necessariamente corresponde ao tamanho lógico do arquivo.

O desafio também reforçou a importância de compreender a sintaxe das ferramentas e consultar a documentação quando necessário.

---

[← Nível anterior](./level-04-05.md) · [Voltar ao índice](../README.md)
