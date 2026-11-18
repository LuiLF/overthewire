# Bandit — Nível 4 → 5

## Objetivo

Encontrar a senha do próximo nível, armazenada no único arquivo legível por humanos dentro do diretório `inhere`.

## Exploração

Após acessar o diretório `inhere`, encontrei dez arquivos, nomeados de `-file00` a `-file09`.

Inicialmente, tentei identificar o arquivo correto lendo seus conteúdos individualmente com `cat`:

```bash
cat -- "-file00"
cat -- "-file01"
cat -- "-file02"
```

A maioria retornava caracteres ilegíveis. Continuei verificando os arquivos até encontrar a senha em `-file07`:

```bash
cat -- "-file07"
```

## Alternativa mais eficiente

Depois de resolver o desafio, explorei uma maneira de identificar o tipo de conteúdo de todos os arquivos de uma vez:

```bash
file -- -file0*
```

O comando `file` analisou os arquivos selecionados pelo curinga `*`, identificando `-file07` como `ASCII text`, enquanto outros foram classificados como `data` ou diferentes tipos de texto.

Essa abordagem evita a leitura manual de cada arquivo e se torna especialmente útil quando há muitos arquivos para analisar.

## Resultado

A senha foi encontrada em `-file07`, permitindo avançar para o próximo nível.

---

[← Nível anterior](./level-03-04.md) · [Voltar ao índice](../README.md)
