# Bandit — Nível 10 → 11

## Objetivo

Encontrar a senha do próximo nível no arquivo `data.txt`, cujo conteúdo está codificado em Base64.

## Exploração

Inicialmente, listei os arquivos disponíveis:

```bash
ls
```

O diretório continha o arquivo `data.txt`.

Como já conhecia Base64 de laboratórios de cibersegurança e CTFs, identifiquei o comando `base64` como uma possível ferramenta para resolver o desafio.

Primeiro, executei:

```bash
base64 data.txt
```

O comando retornou outra sequência codificada, pois, por padrão, `base64` realiza a codificação dos dados.

Consultei a documentação do comando e identifiquei a opção `-d`, utilizada para decodificação.

## Resolução

Executei:

```bash
base64 -d data.txt
```

O comando decodificou o conteúdo do arquivo e exibiu a senha do próximo nível em texto legível.

## Observações

- O comando `base64`, sem opções, codifica os dados recebidos.
- A opção `-d` (`--decode`) realiza a decodificação.

---

[← Nível anterior](level-09-10.md) · [Voltar ao índice](../README.md)
