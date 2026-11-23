# Bandit 31 → 32 — Git Push, .gitignore e Remote Hooks

## Objetivo

Criar um arquivo com nome e conteúdo específicos, adicioná-lo ao controle de versão e enviá-lo ao repositório Git remoto para validação.

## Investigação

Após clonar o repositório via SSH e acessar seu diretório, foram identificados dois arquivos:

```text
.gitignore
README.md
```

O arquivo `README.md` apresentava as instruções do desafio:

```text
File name: key.txt
Content: 'May I come in?'
Branch: master
```

O objetivo era criar o arquivo `key.txt` com o conteúdo `May I come in?` e enviá-lo para a branch `master` do repositório remoto.

### 1. Análise do .gitignore

O conteúdo do `.gitignore` foi consultado:

```powershell
cat .\.gitignore
```

Resultado:

```gitignore
*.txt
```

Essa regra faz com que o Git ignore arquivos não rastreados com a extensão `.txt`, incluindo o arquivo solicitado pelo desafio.

Para adicionar intencionalmente um arquivo ignorado, foi identificada a opção `-f` (`--force`) do comando `git add`.

### 2. Verificação do repositório remoto

Antes de enviar as alterações, foi verificada a configuração do remoto:

```powershell
git remote -v
```

O resultado confirmou que `origin` estava configurado para operações de `fetch` e `push`, utilizando o endereço SSH do repositório do desafio.

## Resolução

### 1. Adição do arquivo

Após criar `key.txt` com o conteúdo solicitado, o arquivo foi adicionado à área de staging:

```powershell
git add -f key.txt
```

A opção `-f` permitiu adicionar o arquivo apesar da regra presente no `.gitignore`.

### 2. Configuração da identidade Git

A primeira tentativa de commit retornou o erro:

```text
Author identity unknown
fatal: unable to auto-detect email address
```

O Git exigia um nome e um e-mail para registrar a autoria do commit.

A identidade foi configurada apenas para o repositório atual:

```powershell
git config user.name "Lui"
git config user.email "email@example.com"
```

A configuração local foi utilizada sem `--global`, evitando alterar a identidade dos demais repositórios.

### 3. Criação do commit

Com a identidade configurada, o commit foi criado:

```powershell
git commit -m "add key file"
```

Resultado:

```text
[master 1f39272] add key file
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
```

O Git confirmou a criação do commit contendo o novo arquivo.

### 4. Envio ao repositório remoto

O commit foi enviado com:

```powershell
git push origin master
```

Durante a operação, o servidor executou uma validação:

```text
remote: ### Attempting to validate files... ####
remote:
remote: Well done! Here is the password for the next level:
remote: [SENHA]
```

A senha do próximo nível foi exibida, confirmando que o arquivo atendia aos requisitos.

Entretanto, a operação terminou com:

```text
! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs
```

Apesar da rejeição do push, o desafio foi concluído com sucesso, pois a validação remota confirmou o conteúdo enviado.

## Observações técnicas

### Área de staging, commits e push

O fluxo básico de versionamento com Git envolve três operações:

```bash
git add
git commit
git push
```

- `git add`: prepara alterações para o próximo commit.
- `git commit`: registra as alterações no histórico local.
- `git push`: tenta enviar os commits ao repositório remoto.

O comando `git add` não envia arquivos diretamente ao servidor.

### .gitignore e git add --force

O `.gitignore` define padrões de arquivos não rastreados que o Git deve ignorar.

A opção `-f` permite adicionar explicitamente um arquivo ignorado, sem necessidade de modificar ou excluir o `.gitignore`.

### Configuração de autoria

O Git utiliza `user.name` e `user.email` como metadados de autoria dos commits.

A configuração pode ser local, limitada ao repositório atual, ou global, aplicada como padrão aos repositórios do usuário.

Esses dados não substituem a autenticação SSH utilizada para acessar o servidor.

### Git Remote e origin

O comando `git remote -v` permite consultar os endereços configurados para operações remotas.

O nome `origin` é normalmente criado automaticamente durante o `git clone`, identificando o repositório de origem.

### Pre-receive Hooks

Um `pre-receive hook` é um mecanismo executado no servidor durante o processamento de um `git push`, antes da aceitação das atualizações das referências remotas.

Ele pode validar alterações e rejeitar o envio quando determinadas condições não são atendidas.

Neste desafio, o servidor validou o arquivo e exibiu a senha, mas rejeitou a atualização da branch remota.

Isso demonstra que uma validação pode ser bem-sucedida mesmo quando a operação de push termina com erro.

## Conceitos aprendidos

- Criação e versionamento de arquivos
- Regras do `.gitignore`
- Adição forçada com `git add -f`
- Área de staging
- Criação de commits com `git commit -m`
- Configuração local de identidade Git
- Verificação de remotos com `git remote -v`
- Envio de commits com `git push`
- Validação remota e `pre-receive hooks`

---

[← Nível anterior](level-30-31.md) · [Voltar ao índice](../README.md)
