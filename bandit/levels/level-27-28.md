# Bandit 27 → 28 — Git Clone via SSH

## Objetivo

Clonar um repositório Git remoto utilizando SSH, a partir da máquina local, e localizar a senha do próximo nível.

O repositório está disponível na porta `2220` do servidor OverTheWire, utilizando as credenciais do usuário `bandit27`.

## Investigação

O desafio exigia utilizar o Git localmente, em vez de executar os comandos dentro do servidor Bandit.

Após instalar o Git for Windows, foi realizada uma tentativa inicial de clonagem pelo PowerShell:

```powershell
git clone ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo -p 2220
```

O comando retornou:

```text
error: unknown switch `p'
```

O erro ocorreu porque `-p` é uma opção do cliente SSH, não do comando `git clone`.

A porta foi então incluída diretamente na URL SSH:

```powershell
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Após a autenticação, a clonagem foi concluída com sucesso.

## Resolução

### 1. Localização do repositório clonado

Como nenhum diretório de destino foi informado, o Git criou automaticamente uma pasta chamada `repo` no diretório atual do PowerShell.

```powershell
cd repo
ls
```

O resultado apresentou um arquivo chamado `README`.

Para verificar também os itens ocultos:

```powershell
ls -Force
```

Foi identificado o diretório `.git`, responsável por armazenar os metadados e o histórico do repositório.

### 2. Leitura do arquivo

O conteúdo do arquivo foi consultado diretamente pelo PowerShell:

```powershell
cat .\README
```

```text
The password to the next level is: [SENHA]
```

A senha do próximo nível estava armazenada no próprio arquivo `README`.

## Observações técnicas

### Git e SSH

O comando `git clone` cria uma cópia local de um repositório remoto, incluindo seus arquivos e histórico de versionamento.

A sintaxe geral é:

```bash
git clone <URL_DO_REPOSITORIO> [DIRETORIO_DESTINO]
```

Quando o diretório de destino é omitido, o Git utiliza normalmente o nome do repositório remoto.

Em URLs SSH, uma porta personalizada pode ser informada após o endereço do servidor:

```text
ssh://usuario@servidor:porta/caminho/repositorio
```

### PowerShell e diretório `.git`

No PowerShell, `cat` é um alias de `Get-Content`, utilizado para ler o conteúdo de arquivos.

O parâmetro `-Force` de `Get-ChildItem` (`ls`) permite exibir itens ocultos.

O diretório `.git` contém informações utilizadas pelo Git para controlar o versionamento. O ponto inicial faz parte do nome do diretório, não representa uma extensão de arquivo.

## Conceitos aprendidos

- Instalação e utilização do Git no Windows
- Clonagem de repositórios com `git clone`
- Autenticação Git via SSH
- Configuração de porta personalizada em URLs SSH
- Diretório de trabalho e destino de clonagem
- Navegação e leitura de arquivos no PowerShell
- Estrutura básica de um repositório Git

---

[← Nível anterior](level-26-27.md) · [Voltar ao índice](../README.md)
