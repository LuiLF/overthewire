# Bandit 32 → 33 — Uppercase Shell Escape

## Objetivo

Escapar de um shell restrito que converte os comandos digitados para letras maiúsculas e recuperar a senha do próximo usuário.

## Investigação

Após conectar ao servidor via SSH, foi apresentado um terminal diferente do convencional:

```text
WELCOME TO THE UPPERCASE SHELL
>>
```

A primeira tentativa de listar os arquivos retornou um erro:

```bash
>> ls
sh: 1: LS: Permission denied
```

O comando `ls`, digitado em minúsculas, foi interpretado como `LS`.

Uma segunda tentativa utilizando letras maiúsculas produziu o mesmo resultado:

```bash
>> LS
sh: 1: LS: Permission denied
```

Isso indicou que o ambiente estava convertendo automaticamente os comandos para maiúsculas antes de executá-los.

Como o Linux diferencia letras maiúsculas e minúsculas, comandos convencionais não funcionavam normalmente.

### 1. Investigação de variáveis

Inicialmente, foram realizados testes com variáveis do shell:

```bash
>> CAT=ls
>> CAT
sh: 1: CAT: Permission denied
>> $CAT
>>
```

A utilização de `$CAT` apresentou um comportamento diferente, levando à investigação de expansões de variáveis.

Entretanto, atribuir comandos diretamente não resolvia o problema, pois o Uppercase Shell também convertia o conteúdo digitado.

### 2. Investigação das variáveis de ambiente

Foram testadas variáveis normalmente disponibilizadas pelo ambiente Linux:

```bash
>> $home
sh: 1: /home/bandit32: Permission denied

>> $user
sh: 1: bandit32: Permission denied

>> $path
sh: 1: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin: not found

>> $shell
WELCOME TO THE UPPERCASE SHELL
```

Os testes demonstraram que a expansão de variáveis funcionava, mesmo quando a entrada era convertida para maiúsculas.

Os valores das variáveis eram substituídos pelo shell antes da tentativa de execução.

A variável `$SHELL`, entretanto, iniciava novamente o ambiente restrito.

## Resolução

A investigação levou à utilização do parâmetro especial `$0`:

```bash
>> $0
```

Após sua execução, o terminal passou a aceitar comandos convencionais:

```bash
$ pwd
/home/bandit32

$ ls
uppershell
```

O parâmetro `$0` é utilizado pelo shell para representar seu nome de invocação ou o nome do script em execução.

Nesse ambiente, sua expansão permitiu iniciar um interpretador de comandos sem a restrição de conversão para maiúsculas.

### 1. Análise do executável

Com o shell funcional, foi investigado o arquivo `uppershell`:

```bash
file uppershell
```

Resultado:

```text
uppershell: setuid ELF 32-bit LSB executable, Intel i386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=fb9a82a6921a8788d50baa8f8dadd8d35354b5bf, for GNU/Linux 3.2.0, not stripped
```

O resultado indicou que `uppershell` era um executável ELF de 32 bits com o atributo SUID.

Uma tentativa de ler o arquivo com `cat` apresentou conteúdo binário, confirmando que não se tratava de um script de texto.

### 2. Recuperação da senha

Após escapar do shell restrito, foi acessado o diretório de senhas utilizado nos níveis anteriores:

```bash
cd /etc/bandit_pass/
```

Os arquivos disponíveis foram listados:

```bash
ls
```

Em seguida, foi consultado o arquivo correspondente ao próximo usuário:

```bash
cat bandit33
```

Resultado:

```text
[SENHA]
```

A senha foi recuperada, concluindo o último desafio jogável do OverTheWire Bandit.

## Observações técnicas

### Shell restrito

Um shell restrito limita as operações disponíveis ao usuário.

Neste desafio, o Uppercase Shell convertia a entrada para maiúsculas, impedindo a execução direta de diversos comandos convencionais.

A restrição, entretanto, não impedia a utilização de expansões interpretadas pelo shell subjacente.

### Expansão de variáveis

O shell permite acessar valores armazenados em variáveis utilizando a sintaxe:

```bash
$VARIAVEL
```

Quando uma expansão aparece na posição de comando, o valor resultante pode ser interpretado como um comando executável.

As variáveis de ambiente podem ser herdadas de processos anteriores, sem necessidade de serem definidas manualmente pelo usuário.

### Parâmetro especial $0

O parâmetro `$0` representa o nome utilizado para invocar o shell ou script atual.

Neste desafio, sua execução permitiu iniciar outro shell e contornar a restrição imposta pelo Uppercase Shell.

O comportamento depende do ambiente e não representa um método universal para escapar de shells restritos.

### Arquivos ELF e SUID

ELF (Executable and Linkable Format) é um formato utilizado por executáveis e outros arquivos binários em sistemas Linux.

O atributo SUID permite que um executável seja iniciado com o UID efetivo de seu proprietário, sujeito às regras e restrições do sistema.

A presença desse atributo não significa, por si só, que qualquer usuário terá acesso irrestrito ao sistema.

### Shell Escape

Um shell escape consiste em obter acesso a um interpretador de comandos fora das restrições de um ambiente inicialmente disponibilizado.

Neste caso, a exploração utilizou a expansão de um parâmetro especial do shell.

O desafio reforçou a importância de compreender a ordem de interpretação dos comandos e os mecanismos de expansão do shell.

## Conceitos aprendidos

- Shells restritos
- Sensibilidade a maiúsculas e minúsculas no Linux
- Expansão de variáveis de ambiente
- Parâmetros especiais do shell (`$0`)
- Shell escape
- Identificação de executáveis ELF
- Atributo SUID
- Investigação de permissões e acesso a arquivos

---

[← Nível anterior](level-31-32.md) · [Voltar ao índice](../README.md)
