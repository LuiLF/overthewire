# Bandit 28 → 29 — Git History e recuperação de informações

## Objetivo

Clonar um repositório Git remoto e investigar seu histórico de alterações para encontrar a senha do próximo nível.

## Investigação

O repositório foi clonado via SSH, utilizando a porta `2220`:

```powershell
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo Downloads/repo
```

Após acessar o diretório clonado, foi consultado o arquivo `README.md`:

```powershell
cd Downloads/repo
cat .\README.md
```

O conteúdo apresentava as credenciais do próximo usuário, mas a senha estava ocultada:

```text
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

Como o Git mantém o histórico de alterações dos arquivos, foi investigado o histórico do `README.md`:

```powershell
git log .\README.md
```

Foram identificados três commits:

| Commit | Mensagem |
|---|---|
| `eaed925` | fix info leak |
| `26663cf` | add missing data |
| `40f8253` | initial commit of README.md |

A mensagem `fix info leak` indicava que uma informação havia sido removida para corrigir um vazamento. Isso motivou a investigação do commit anterior.

## Resolução

Inicialmente, foi utilizado `git restore` com o hash do commit mais recente:

```powershell
git restore .\README.md --source eaed925
```

O comando foi executado sem erros, mas não produziu alterações perceptíveis, pois restaurava a mesma versão já presente no diretório.

Após identificar que o commit `26663cf` antecedia a correção do vazamento, foi executado:

```powershell
git restore .\README.md --source 26663cf
```

O comando substituiu o conteúdo local do arquivo pela versão armazenada naquele commit.

A leitura do arquivo confirmou a alteração:

```powershell
cat .\README.md
```

```text
## credentials

- username: bandit29
- password: [SENHA]
```

A senha do próximo nível foi recuperada a partir de uma versão anterior do arquivo.

## Observações técnicas

### Histórico de commits

O Git registra versões do projeto por meio de commits, identificados por hashes.

O comando `git log` permite consultar esse histórico, incluindo autor, data e mensagem de cada alteração.

É possível restringir a consulta ao histórico de um arquivo específico:

```bash
git log README.md
```

### Recuperação de versões

O comando `git restore --source` permite recuperar o conteúdo de um arquivo a partir de um commit específico.

```bash
git restore --source <commit> <arquivo>
```

A operação substitui o conteúdo do arquivo na pasta de trabalho, sem criar uma segunda cópia e sem apagar o histórico existente.

Para visualizar uma versão antiga sem modificar o arquivo local, também é possível utilizar:

```bash
git show <commit>:<arquivo>
```

### Exposição de informações sensíveis

Remover credenciais da versão atual de um arquivo não garante sua remoção do histórico Git.

Se uma senha ou chave secreta for publicada em um commit, ela poderá continuar acessível em versões anteriores do repositório.

Em ambientes reais, credenciais expostas devem ser revogadas ou substituídas, mesmo após sua remoção do código atual.

## Conceitos aprendidos

- Histórico de versionamento com Git
- Identificação e interpretação de commits
- Utilização de `git log`
- Recuperação de versões com `git restore`
- Consulta de versões antigas com `git show`
- Diferença entre histórico Git e pasta de trabalho
- Riscos de exposição de credenciais em repositórios

---

[← Nível anterior](level-27-28.md) · [Voltar ao índice](../README.md)
