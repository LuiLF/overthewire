# Bandit — Nível 21 → 22

## Objetivo

Investigar uma tarefa agendada pelo `cron`, identificar o comando executado automaticamente e compreender seu funcionamento para encontrar a senha do próximo nível.

## Exploração

Inicialmente, tentei executar o serviço `cron` diretamente:

```bash
cron
```

O comando retornou:

```text
cron: can't open or create /var/run/crond.pid: Permission denied
```

Depois, acessei o diretório indicado pelo enunciado e listei seus arquivos:

```bash
cd /etc/cron.d/
ls
```

Entre os arquivos disponíveis, identifiquei `cronjob_bandit22`.

Consultei a documentação com `man 5 crontab` para compreender a estrutura das tarefas agendadas e, em seguida, li sua configuração:

```bash
cat cronjob_bandit22
```

O conteúdo encontrado foi:

```text
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

A configuração indica que o script `/usr/bin/cronjob_bandit22.sh` é executado como usuário `bandit22`, tanto na inicialização do serviço cron quanto a cada minuto.

O redirecionamento `&> /dev/null` descarta a saída padrão e os erros produzidos pelo comando.

### Investigação do script

Após identificar o caminho do script, acessei `/usr/bin/` e examinei seu conteúdo:

```bash
cd /usr/bin/
cat cronjob_bandit22.sh
```

O script continha:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Inicialmente, tive dúvida se a sequência de caracteres no caminho `/tmp/` seria a própria senha, pois tinha o mesmo comprimento das senhas anteriores.

Ao analisar os comandos, compreendi que ela era apenas o nome de um arquivo.

O script configura suas permissões com `chmod 644` e copia o conteúdo de `/etc/bandit_pass/bandit22` para esse arquivo temporário.

## Resolução

Primeiro, tentei acessar diretamente o arquivo protegido:

```bash
cat /etc/bandit_pass/bandit22
```

Resultado:

```text
Permission denied
```

Em seguida, tentei executar o arquivo localizado em `/tmp/`:

```bash
/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Também recebi `Permission denied`, pois estava tentando executar um arquivo que não possuía permissão de execução para meu usuário.

Por fim, utilizei `cat` para ler seu conteúdo:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

O comando retornou a senha do nível 22.

**Nível concluído.**

## Observações

- **Cron:** serviço responsável por executar tarefas automaticamente em horários ou intervalos definidos.
- **`/etc/cron.d/`:** diretório que contém arquivos de configuração de tarefas agendadas do sistema.
- **`* * * * *`:** representa uma execução a cada minuto, e não um processo executando continuamente.
- **`@reboot`:** agenda a execução quando o serviço cron inicia.
- **`chmod 644`:** permite leitura e escrita ao proprietário e somente leitura aos demais usuários.
- **Redirecionamento `>`:** envia a saída de um comando para um arquivo, substituindo seu conteúdo.
- **`/dev/null`:** dispositivo especial utilizado para descartar dados.
- **Caminhos absolutos:** permitem acessar um arquivo diretamente, sem precisar mudar para o diretório onde ele está armazenado.

### Aprendizado

A principal dificuldade foi compreender como investigar as configurações do cron e identificar quais arquivos poderiam ser lidos com `cat`, mesmo sem extensões como `.txt`.

Aprendi que arquivos de configuração no Linux frequentemente são textos legíveis sem extensão e que o comando `file` pode ajudar a identificar o tipo de conteúdo antes de tentar interpretá-lo.

Também compreendi a diferença entre o agendamento de uma tarefa, o script executado por ela e os arquivos que esse script manipula.

---

[← Nível anterior](level-20-21.md) · [Voltar ao índice](../README.md)
