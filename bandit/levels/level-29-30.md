# Bandit 29 → 30 — Git Branches e análise de commits

## Objetivo

Investigar um repositório Git remoto para localizar a senha do próximo nível, explorando branches e diferenças entre versões do projeto.

## Investigação

O repositório foi clonado via SSH, utilizando a porta `2220`:

```powershell
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo bandit29
```

Após acessar o diretório clonado, foi consultado o arquivo `README.md`:

```powershell
cd bandit29
cat .\README.md
```

O conteúdo apresentava as credenciais do próximo usuário, mas não revelava a senha:

```text
## credentials

- username: bandit30
- password: <no passwords in production!>
```

### 1. Investigação do histórico

Inicialmente, foi utilizada a mesma abordagem do nível anterior:

```powershell
git log
```

Foram identificados dois commits na branch `master`:

| Commit | Mensagem |
|---|---|
| `bf3f6e5` | fix username |
| `9682add` | initial commit of README.md |

A versão inicial foi recuperada utilizando:

```powershell
git restore .\README.md --source 9682add
```

O arquivo apresentou uma alteração no nome do usuário, mas a senha continuava indisponível.

### 2. Investigação das branches

Como o histórico da `master` não apresentou a informação desejada, foram investigadas as branches disponíveis:

```powershell
git branch -a
```

Resultado:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

O comando revelou outras duas branches remotas: `dev` e `sploits-dev`.

Uma tentativa de consultar diretamente `dev` com `git log dev` retornou erro, pois não existia uma branch local com esse nome.

As referências remotas precisam ser identificadas pelo prefixo `origin/`, como em `origin/dev`.

## Resolução

Inicialmente, foi realizada uma tentativa de acessar a branch de desenvolvimento:

```powershell
git switch --track origin/dev
```

O Git impediu a operação porque o arquivo `README.md` possuía alterações locais que seriam sobrescritas pela troca de branch.

Em seguida, foi investigada a referência remota diretamente:

```powershell
git show origin/dev
```

O comando apresentou um commit com a mensagem:

```text
add data needed for development
```

O diff desse commit revelou uma alteração no campo de senha do `README.md`:

```diff
  ## credentials

  - username: bandit30
- - password: <no passwords in production!>
+ - password: [SENHA]
```

A senha do próximo nível foi identificada na linha adicionada, sem necessidade de trocar de branch ou modificar os arquivos locais.

## Observações técnicas

### Branches locais e remotas

Branches permitem manter linhas de desenvolvimento distintas dentro de um mesmo repositório Git.

Os comandos utilizados para investigá-las incluem:

```bash
git branch       # Lista branches locais
git branch -r    # Lista referências de branches remotas
git branch -a    # Lista ambas
```

O prefixo `origin/` identifica referências de acompanhamento das branches remotas.

### Consulta de commits e arquivos

O comando `git show` permite examinar um commit e suas alterações:

```bash
git show origin/dev
```

Também é possível consultar diretamente um arquivo de determinada branch, sem modificar a pasta de trabalho:

```bash
git show origin/dev:README.md
```

### Interpretação de diffs

No formato de diff do Git:

- `-` indica conteúdo removido.
- `+` indica conteúdo adicionado.
- Linhas de contexto mostram partes do arquivo que não foram alteradas.

### Proteção das alterações locais

O Git pode impedir uma troca de branch quando a operação sobrescreveria alterações locais.

Isso evita a perda acidental de trabalho não registrado.

### Segurança em repositórios Git

Informações sensíveis podem permanecer acessíveis em branches secundárias, mesmo quando não estão presentes na branch principal.

Uma auditoria de segurança deve considerar o histórico e as diferentes referências do repositório, não apenas os arquivos atualmente visíveis na `master`.

## Conceitos aprendidos

- Branches locais e remotas
- Referências `origin/` e `HEAD`
- Listagem de branches com `git branch -a`
- Consulta de commits com `git show`
- Interpretação de diffs
- Diferença entre alterações locais e conteúdo versionado
- Exposição de credenciais em branches secundárias

---

[← Nível anterior](level-28-29.md) · [Voltar ao índice](../README.md)
