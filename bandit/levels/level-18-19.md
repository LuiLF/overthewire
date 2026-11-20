# Bandit — Nível 18 → 19

## Objetivo

Encontrar a senha do próximo nível, armazenada no arquivo `readme` no diretório inicial, apesar de uma modificação no `.bashrc` que encerra a sessão ao tentar acessar o servidor via SSH.

## Exploração

Inicialmente, tentei estabelecer uma conexão SSH normalmente:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

A autenticação foi realizada, mas a sessão foi encerrada automaticamente com a mensagem:

```text
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

Sabendo que o problema estava relacionado à inicialização do shell, consultei a documentação do SSH em busca de uma maneira de executar comandos remotamente sem precisar manter uma sessão interativa.

Descobri que o SSH permite informar um comando diretamente após o endereço do servidor.

Minha primeira tentativa foi:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat README
```

O servidor respondeu:

```text
cat: README: No such file or directory
```

Para verificar o nome correto do arquivo, executei:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 ls
```

Resultado:

```text
readme
```

O problema era a diferença entre letras maiúsculas e minúsculas no nome do arquivo.

## Resolução

Com o nome correto, executei:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

O comando retornou o conteúdo do arquivo, permitindo recuperar a senha do próximo nível sem iniciar uma sessão interativa.

## Observações

- O SSH permite executar comandos remotamente sem abrir uma sessão interativa.
- Ao informar um comando após o endereço do servidor, o SSH realiza a autenticação, executa o comando remoto, retorna sua saída e encerra a conexão.
- O `.bashrc` é um arquivo de configuração do Bash, normalmente utilizado para configurar sessões interativas.
- A execução remota de um comando pode evitar configurações que afetam apenas a inicialização de shells interativos.
- Sistemas Linux geralmente diferenciam letras maiúsculas de minúsculas nos nomes dos arquivos (`README` e `readme` são nomes diferentes).
- A execução remota de comandos por SSH é útil para administração de servidores e automação de tarefas.

---

[← Nível anterior](level-17-18.md) · [Voltar ao índice](../README.md)
