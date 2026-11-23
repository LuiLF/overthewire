# Bandit 30 → 31 — Git Tags

## Objetivo

Investigar um repositório Git remoto para encontrar a senha do próximo nível, explorando referências além de commits e branches.

## Investigação

O repositório foi clonado via SSH, utilizando a porta `2220`:

```powershell
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo bandit30
```

Após acessar o diretório, foi consultado o arquivo `README.md`:

```powershell
cd bandit30
cat .\README.md
```

O arquivo apresentava apenas a mensagem:

```text
just an epmty file... muahaha
```

### 1. Investigação do histórico

Inicialmente, foi consultado o histórico de commits:

```powershell
git log
```

O resultado apresentou apenas um commit:

```text
1f03ad2 initial commit of README.md
```

Em seguida, foram verificadas as branches disponíveis:

```powershell
git branch -a
```

Resultado:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

As referências disponíveis apontavam para a mesma linha de histórico, sem indicar branches adicionais.

O conteúdo do commit também foi examinado:

```powershell
git show origin/master
```

O diff confirmou que o commit apenas adicionava o `README.md`, sem revelar informações adicionais.

## Resolução

Como a investigação de commits e branches não apresentou resultados, foram exploradas outras formas de referenciar objetos no Git.

Foi utilizado o comando:

```powershell
git tag -l
```

Resultado:

```text
secret
```

A tag encontrada foi então examinada:

```powershell
git show secret
```

O comando exibiu diretamente a senha necessária para acessar o próximo nível:

```text
[SENHA]
```

A investigação foi concluída sem modificar os arquivos do repositório.

## Observações técnicas

### Git Tags

Tags são referências utilizadas para identificar pontos específicos do histórico ou outros objetos do Git.

Elas são frequentemente utilizadas para marcar versões de software, como `v1.0.0` e `v2.0.0`.

```bash
git tag -l
```

Lista as tags disponíveis no repositório.

```bash
git show <tag>
```

Exibe informações sobre o objeto referenciado pela tag.

Diferentemente das branches, tags normalmente não avançam automaticamente quando novos commits são criados.

### Git Tags e GitHub Releases

Uma tag é um recurso nativo do Git, enquanto um Release é um recurso disponibilizado pelo GitHub.

No GitHub, Releases são associados a tags e podem incluir notas de versão, instruções e arquivos para distribuição.

Assim, uma tag pode existir sem um Release, mas um Release normalmente utiliza uma tag para identificar a versão publicada.

### Referências e segurança

A investigação de um repositório não deve se limitar à branch principal.

Além de commits e branches, tags também podem referenciar objetos que contenham informações relevantes ou sensíveis.

## Conceitos aprendidos

- Git Tags
- Listagem de tags com `git tag -l`
- Inspeção de objetos com `git show`
- Diferença entre branches e tags
- Relação entre Git Tags e GitHub Releases
- Investigação de referências Git além da branch principal

---

[← Nível anterior](level-29-30.md) · [Voltar ao índice](../README.md)
