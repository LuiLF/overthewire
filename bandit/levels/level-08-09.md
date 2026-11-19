# Bandit — Nível 8 → 9

## Objetivo

Encontrar a senha do próximo nível no arquivo `data.txt`, identificando a única linha que aparece apenas uma vez.

## Exploração

Inicialmente, utilizei `ls` para verificar os arquivos disponíveis e `cat` para visualizar o conteúdo:

```bash
ls
cat data.txt
```

O arquivo continha diversas linhas de texto, muitas delas repetidas.

Como o objetivo era encontrar uma linha única, consultei a documentação do Ubuntu para entender o funcionamento do comando `uniq`. Nos exemplos, observei sua utilização em conjunto com `sort`.

Minha primeira tentativa foi:

```bash
sort data.txt | uniq
```

O comando organizou as linhas e eliminou as repetições consecutivas, mas ainda retornou várias linhas distintas, sem identificar especificamente aquela que aparecia apenas uma vez.

Após consultar novamente os exemplos da documentação, encontrei a opção `-u`, utilizada para exibir somente linhas com uma única ocorrência.

## Resolução

Executei:

```bash
sort data.txt | uniq -u
```

O comando retornou apenas uma linha, correspondente à senha do próximo nível.

O funcionamento da sequência é:

- `sort data.txt`: organiza as linhas do arquivo, agrupando as iguais.
- `|`: encaminha a saída do primeiro comando para a entrada do seguinte.
- `uniq -u`: exibe somente as linhas que aparecem exatamente uma vez.

## Observações

- O `uniq` compara linhas consecutivas, por isso o uso de `sort` é importante quando as repetições estão espalhadas pelo arquivo.
- Sem opções, `uniq` mantém uma ocorrência de cada grupo de linhas repetidas.
- A opção `-u` (ou `--unique`) exibe apenas linhas que não possuem repetições.
- O operador `|` permite combinar comandos sem criar arquivos intermediários.

---

[← Nível anterior](level-07-08.md) · [Voltar ao índice](../README.md)
