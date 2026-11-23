# Bandit 25 → 26 — Restricted Shell Escape

## Objetivo

Acessar o usuário `bandit26`, que utiliza um shell de login diferente do Bash, compreender seu funcionamento e encontrar uma maneira de executar comandos no ambiente restrito.

## Investigação

No diretório pessoal do `bandit25`, foi encontrada uma chave privada SSH:

```bash
ls
# bandit26.sshkey
```

A chave foi transferida para o Windows utilizando `scp` e utilizada para autenticação:

```cmd
ssh -i Downloads/bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

A autenticação funcionou, mas a conexão foi encerrada após a exibição do banner do servidor.

Para investigar o shell configurado para o usuário, foi consultado o arquivo `/etc/passwd`:

```bash
grep '^bandit26:' /etc/passwd
```

O resultado indicou `/usr/bin/showtext` como shell de login.

A análise do arquivo revelou que se tratava de um script:

```bash
file /usr/bin/showtext
cat /usr/bin/showtext
```

```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

O script utiliza `exec` para substituir seu processo pelo `more`, responsável por exibir o arquivo `text.txt`. Quando o visualizador termina, a sessão SSH também é encerrada.

## Resolução

### 1. Investigação do comportamento do `more`

O comando `more` permite visualizar arquivos de forma paginada, pausando a exibição quando o conteúdo ultrapassa o espaço disponível no terminal.

Ao reduzir a altura da janela do CMD antes da conexão SSH, foi possível fazer com que o `more` permanecesse aberto aguardando interação.

### 2. Acesso ao Vim

A ajuda interativa do `more`, acessada com `h`, apresentou a opção:

```text
v    Start up '/usr/bin/vi' at current line
```

Utilizando `v`, foi possível abrir o editor Vim dentro da sessão restrita.

### 3. Execução de comandos

O Vim permite executar comandos externos utilizando `:!`, mas inicialmente esses comandos continuavam utilizando o shell `showtext`.

A configuração foi consultada com:

```vim
:set shell?
```

Em seguida, o shell utilizado pelo editor foi alterado para Bash:

```vim
:set shell=/bin/bash
```

Com isso, tornou-se possível executar comandos normalmente:

```vim
:!pwd
:!ls
```

A execução de `pwd` confirmou o diretório `/home/bandit26`, demonstrando que os comandos estavam sendo executados no contexto do usuário autenticado.

## Observações técnicas

- O shell de login é definido na configuração da conta, normalmente registrada em `/etc/passwd`.
- `exec` substitui o processo atual pelo programa especificado.
- O `more` oferece funcionalidades interativas além da simples exibição de texto.
- O Vim permite executar comandos externos e configurar qual shell utilizar.
- Restringir a interface disponível ao usuário não garante, por si só, o isolamento da execução de comandos.

O mecanismo explorado é conhecido como **shell escape**: a utilização de funcionalidades de um programa permitido para executar comandos fora das restrições pretendidas.

## Conceitos aprendidos

- Shell de login e `/etc/passwd`
- Scripts POSIX e `exec`
- Paginação e interação com `more`
- Modos e comandos do Vim
- Shell escape e restrições de ambiente

---

[← Nível anterior](level-24-25.md) · [Voltar ao índice](../README.md)
