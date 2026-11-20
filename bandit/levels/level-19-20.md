# Bandit — Nível 19 → 20

## Objetivo

Utilizar um executável com a permissão especial SetUID, localizado no diretório inicial, para acessar a senha do próximo nível em `/etc/bandit_pass`.

## Exploração

Inicialmente, listei os arquivos disponíveis:

```bash
ls
```

Resultado:

```text
bandit20-do
```

Como ainda não compreendia completamente o funcionamento do SetUID, tentei utilizá-lo como se fosse um comando:

```bash
setuid
setuid bandit20-do
setuid bandit20-do cat
```

Todas as tentativas retornaram `Command 'setuid' not found`.

Também tentei visualizar seu conteúdo com cat, mas a saída apresentou caracteres ilegíveis, pois o arquivo contém dados binários, e não texto comum.

Para identificar o tipo do arquivo bandit20-do, utilizei o comando file, que indicou se tratar de um executável ELF.

Após compreender que SetUID é um mecanismo de permissões, e não um comando, executei o programa diretamente:

```bash
./bandit20-do
```

O programa apresentou as instruções de uso:

```text
Run a command as another user.
  Example: ./bandit20-do whoami
```

Em seguida, tentei acessar o diretório das senhas:

```bash
./bandit20-do cat /etc/bandit_pass
```

O resultado foi:

```text
cat: /etc/bandit_pass: Is a directory
```

Também tentei mudar de diretório:

```bash
./bandit20-do cd /etc/bandit_pass
```

Mas recebi:

```text
env: 'cd': Permission denied
```

Isso ocorreu porque `cd` é um comando interno do shell, e não um executável independente que o programa pudesse iniciar dessa maneira.

Por fim, listei o conteúdo do diretório:

```bash
./bandit20-do ls /etc/bandit_pass
```

O resultado apresentou diversas entradas com nomes correspondentes aos usuários do Bandit, como `bandit19`, `bandit20` e `bandit21`.

Inicialmente, interpretei essas entradas como subdiretórios, devido à experiência com desafios anteriores. Depois, percebi que o `ls` simples não identifica necessariamente o tipo de cada entrada e que os nomes poderiam corresponder a arquivos.

## Resolução

Sabendo que o objetivo era obter a senha do usuário `bandit20`, utilizei o executável SetUID para ler o arquivo correspondente:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

O comando retornou a senha do próximo nível.

## Observações

- SetUID é uma permissão especial que permite a um executável operar com o UID efetivo de seu proprietário.
- O SetUID não é um comando e não concede automaticamente uma sessão permanente com os privilégios de outro usuário.
- Arquivos ELF são executáveis binários e não devem ser interpretados como arquivos de texto comuns.
- O prefixo `./` permite executar um arquivo localizado no diretório atual.
- `cd` é um comando interno do shell e não pode ser executado diretamente pelo `bandit20-do` como um programa independente.
- O comando `ls` apresenta os nomes das entradas de um diretório, mas não diferencia necessariamente arquivos e diretórios na saída padrão.
- As opções `ls -l` e `ls -F` permitem identificar melhor os tipos de arquivos.
- Executáveis SetUID que permitem executar comandos arbitrários podem representar riscos de escalonamento de privilégios.

---

[← Nível anterior](level-18-19.md) · [Voltar ao índice](../README.md)
