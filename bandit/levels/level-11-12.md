# Bandit — Nível 11 → 12

## Objetivo

Encontrar a senha do próximo nível no arquivo `data.txt`, cujo conteúdo possui todas as letras maiúsculas e minúsculas deslocadas 13 posições no alfabeto (ROT13).

## Exploração

Inicialmente, listei os arquivos disponíveis e visualizei o conteúdo de `data.txt`:

```bash
ls
cat data.txt
```

O arquivo apresentava uma sequência de texto codificada com ROT13.

Minha primeira tentativa foi executar:

```bash
rot13 data.txt
```

Entretanto, o comando não estava disponível no ambiente.

Consultei o material sobre ROT13 indicado pelo próprio desafio e identifiquei uma maneira de realizar a transformação utilizando o comando `tr`.

## Resolução

Utilizei o `tr` para substituir as letras pelos caracteres correspondentes após a rotação de 13 posições:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' <<< "Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA"
```

O comando decodificou o texto e revelou a senha do próximo nível.

## Observações

- O comando `tr` realiza substituições de caracteres com base na correspondência entre dois conjuntos.
- O hífen (`-`) permite representar intervalos, como `A-Z` e `a-z`.
- No ROT13, `N-ZA-M` representa o alfabeto maiúsculo deslocado 13 posições, retornando ao início após `Z`. O mesmo princípio se aplica a `n-za-m`.
- O operador `<<<` envia uma string diretamente para a entrada padrão do comando. Para utilizar o conteúdo de um arquivo, também é possível empregar `<`:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```

---

[← Nível anterior](level-10-11.md) · [Voltar ao índice](../README.md)
