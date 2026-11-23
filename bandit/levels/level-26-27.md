# Bandit 26 → 27 — SUID e privilégios efetivos

## Objetivo

Investigar o executável disponível no ambiente do `bandit26` e utilizá-lo para obter acesso à senha do próximo nível.

## Investigação

Após conseguir executar comandos como `bandit26`, foi listado o conteúdo do diretório pessoal:

```bash
ls
```

```text
bandit27-do  text.txt
```

O arquivo `bandit27-do` foi inicialmente examinado como texto, mas seu conteúdo indicou que se tratava de um executável binário no formato ELF.

Uma tentativa de executá-lo com Bash falhou:

```bash
bash bandit27-do
```

```text
cannot execute binary file
```

O Bash não interpreta diretamente executáveis ELF como scripts.

Ao executar o programa diretamente:

```bash
./bandit27-do
```

Foi apresentada a mensagem:

```text
Run a command as another user.
  Example: ./bandit27-do id
```

Isso indicou que o programa esperava receber um comando como argumento.

## Resolução

### 1. Identificação dos privilégios

Foi utilizado o comando `id` por meio do executável:

```bash
./bandit27-do id
```

Resultado:

```text
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)
```

O resultado demonstrou que o processo mantinha o UID real do `bandit26`, mas utilizava o UID efetivo do `bandit27`.

### 2. Teste de acesso ao arquivo protegido

A tentativa de ler diretamente a senha do próximo usuário retornou um erro:

```bash
cat /etc/bandit_pass/bandit27
```

```text
Permission denied
```

Isso ocorreu porque o comando foi executado com as permissões normais do `bandit26`.

### 3. Execução com privilégios efetivos do próximo usuário

Compreendido o funcionamento do executável, o comando de leitura foi passado como argumento:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

```text
[Senha omitida]
```

O comando foi executado com o UID efetivo do `bandit27`, permitindo a leitura do arquivo protegido.

Durante a resolução, os comandos foram executados a partir do Vim, utilizando o prefixo `:!`, após o shell do editor ter sido configurado para Bash no nível anterior.

## Observações técnicas

### UID e EUID

O Linux diferencia a identidade real do processo da identidade efetiva utilizada em muitas verificações de permissão.

- **UID (User ID):** identifica o usuário real associado ao processo.
- **EUID (Effective User ID):** identifica o usuário cujos privilégios efetivos são utilizados pelo processo.

### SUID

O mecanismo SUID permite que um executável seja iniciado com o UID efetivo de seu proprietário, em vez do usuário que o executou.

O `bandit27-do` utiliza esse mecanismo para permitir a execução de comandos com privilégios do `bandit27`.

Esse comportamento se assemelha, em parte, ao uso de `sudo`, mas os mecanismos de autorização e execução são diferentes.

Em sistemas reais, programas SUID que permitem executar comandos arbitrários podem representar riscos significativos de segurança.

## Conceitos aprendidos

- Diferença entre scripts e programas binários
- UID, GID e EUID
- Permissões Linux
- Mecanismo SUID
- Execução de comandos com privilégios efetivos de outro usuário

---

[← Nível anterior](level-25-26.md) · [Voltar ao índice](../README.md)
