# Bandit — Nível 7 → 8

## Objetivo

Encontrar a senha do próximo nível, armazenada no arquivo `data.txt`, ao lado da palavra `millionth`.

## Exploração

Inicialmente, utilizei `ls` para verificar os arquivos disponíveis no diretório atual:

```bash
ls
```

O comando retornou:

```text
data.txt
```

Como o enunciado informava a palavra que acompanhava a senha, utilizei `grep` para localizar sua ocorrência no arquivo:

```bash
grep millionth data.txt
```

A busca retornou uma linha contendo a palavra `millionth` e a senha correspondente.

## Resolução

O comando `grep` foi suficiente para localizar a informação solicitada, sem necessidade de utilizar outros comandos ou filtros.

Durante a resolução, compreendi que, por padrão, o `grep` exibe a linha inteira que contém o padrão pesquisado, e não apenas a palavra correspondente.

Assim, foi possível identificar a senha diretamente na saída do comando.

## Observações

- O `grep` permite localizar padrões de texto dentro de arquivos.
- Sua sintaxe básica é `grep PADRÃO ARQUIVO`.
- Por padrão, o comando retorna a linha completa que contém o padrão encontrado.
- A opção `-o` permite exibir apenas o trecho correspondente à busca, enquanto `-n` adiciona o número da linha.

---

[← Nível anterior](level-06-07.md) · [Voltar ao índice](../README.md)
