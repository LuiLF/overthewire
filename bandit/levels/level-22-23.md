# Bandit — Nível 22 → 23

## Objetivo

Investigar uma tarefa agendada pelo `cron`, compreender o funcionamento de seu script Bash e localizar o arquivo que contém a senha do próximo nível.

## Exploração

Comecei acessando o diretório de configurações do cron:

```bash
cd /etc/cron.d/
ls
cat cronjob_bandit23
```

A configuração encontrada foi:

```bash
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

O script era executado automaticamente como usuário `bandit23`, tanto na inicialização do cron quanto a cada minuto.

Em seguida, examinei seu conteúdo:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Diferentemente do nível anterior, o arquivo de destino não possuía um nome fixo no código. Seu caminho era construído dinamicamente.

### Investigação das variáveis

Inicialmente, tentei utilizar `$myname` e `$mytarget` diretamente no terminal:

```bash
cat /etc/bandit_pass/$myname
ls /tmp/$mytarget
```

As tentativas falharam porque essas variáveis eram definidas dentro do script e não existiam na minha sessão atual.

Também examinei os arquivos de senha:

```bash
whoami
file /etc/bandit_pass/bandit22
cat /etc/bandit_pass/bandit22
file /etc/bandit_pass/bandit23
```

Confirmei que conseguia ler a senha do usuário atual, mas não tinha permissão para ler diretamente a do próximo nível.

### Entendendo o cálculo do hash

A principal dificuldade estava nesta expressão:

```bash
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
```

Seu funcionamento é:

- `$(...)`: executa os comandos e captura sua saída.
- `echo`: produz a frase contendo o nome do usuário.
- `|`: encaminha a saída de um comando para o seguinte.
- `md5sum`: calcula o hash MD5 da frase.
- `cut -d ' ' -f 1`: extrai apenas o primeiro campo, descartando os espaços e o indicador final da saída do `md5sum`.

O resultado é armazenado em `mytarget` e utilizado como nome de arquivo no diretório `/tmp/`.

## Resolução

Primeiro, testei o cálculo utilizando meu usuário atual:

```bash
echo I am user bandit22 | md5sum | cut -d ' ' -f 1
```

Resultado:

```text
8169b67bd894ddbb4412f91573b38db3
```

Depois, li o arquivo correspondente:

```bash
cat /tmp/8169b67bd894ddbb4412f91573b38db3
```

O conteúdo era a senha do próprio `bandit22`, confirmando que o cálculo correspondia ao funcionamento do script.

A dificuldade restante era compreender o contexto de execução: embora meu terminal estivesse conectado como `bandit22`, a configuração do cron determinava que o script fosse executado como `bandit23`.

Assim, o comando `whoami` dentro do script retornava `bandit23`.

Reproduzi o cálculo com esse valor:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Resultado:

```text
8ca319486bfbbc3663ea0fbe81326349
```

Por fim, acessei o arquivo:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

O arquivo continha a senha necessária para acessar o próximo nível.

## Observações

Durante a investigação, cometi alguns erros que ajudaram a esclarecer conceitos importantes:

- Utilizei variáveis não definidas no meu terminal, confundindo o conteúdo do script com o ambiente da sessão atual.
- Tentei executar diretamente um arquivo de `/tmp/` em vez de ler seu conteúdo com `cat`.
- Testei o redirecionamento `>` no sentido inverso e recebi um erro de permissão ao tentar escrever no arquivo original.
- Inicialmente, interpretei `whoami` considerando apenas meu usuário atual, esquecendo que o cron executava o script como outro usuário.

### Aprendizado

Este nível reforçou conceitos de scripts Bash, substituição de comandos, variáveis, pipes, hashes MD5, extração de campos com `cut` e redirecionamento de saída.

O principal aprendizado foi compreender que **o resultado de um comando depende do contexto em que ele é executado**.

Também ficou evidente a importância de relacionar informações de arquivos diferentes durante uma investigação, especialmente a configuração do cron e o conteúdo do script executado.

---

[← Nível anterior](level-21-22.md) · [Voltar ao índice](../README.md)
