# Bandit — Nível 16 → 17

## Objetivo

Identificar os serviços disponíveis nas portas TCP de `31000` a `32000` do `localhost`, descobrir quais utilizam SSL/TLS e localizar o serviço que fornece as credenciais do próximo nível.

## Exploração

Inicialmente, utilizei o Nmap para identificar as portas abertas no intervalo informado:

```bash
nmap localhost -p 31000-32000
```

O resultado identificou cinco portas abertas, mas não reconheceu os serviços em execução.

Como já conhecia a opção `-sV` de exercícios anteriores no TryHackMe, realizei uma nova varredura com detecção de serviços:

```bash
nmap -sV localhost -p 31000-32000
```

O resultado foi:

```text
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

Inicialmente, escolhi a porta `31518`, identificada como `ssl/echo`, por apresentar um serviço reconhecido pelo Nmap.

Ao tentar enviar a senha com `openssl s_client`, a entrada começava com `k`, acionando o comando interativo `KEYUPDATE` em vez de transmitir o conteúdo normalmente.

Utilizei a opção `-quiet` para evitar a interpretação desses comandos, mas a porta escolhida não forneceu as credenciais esperadas.

Ao revisar o resultado do Nmap, percebi que `echo` identificava serviços que devolviam os dados recebidos. A identificação `ssl/unknown`, por outro lado, indicava que o Nmap reconheceu TLS, mas não conseguiu classificar a aplicação.

## Resolução

Estabeleci uma conexão TLS com a outra porta identificada:

```bash
openssl s_client -quiet -connect 127.0.0.1:31790
```

Após enviar a senha atual, o servidor respondeu:

```text
Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
[conteúdo omitido]
-----END OPENSSH PRIVATE KEY-----
```

A credencial recebida era uma chave privada SSH, em vez de uma senha convencional.

Salvei a chave em um arquivo local no Windows e utilizei o SSH para autenticar no próximo nível:

```powershell
ssh -i Downloads/talvezsenha.txt bandit17@bandit.labs.overthewire.org -p 2220
```

Na primeira tentativa, o SSH retornou `invalid format`. Após investigar o arquivo, identifiquei que faltava uma quebra de linha ao final. Corrigi a formatação, validei a chave com `ssh-keygen -y` e consegui autenticar no Bandit 17.

## Observações

- `nmap` identifica portas abertas; `-sV` realiza sondagens para identificar serviços e versões.
- Uma porta aberta não revela necessariamente qual aplicação está em execução.
- Serviços `echo` devolvem os dados recebidos, enquanto `ssl/echo` utiliza TLS para essa comunicação.
- `ssl/unknown` indica que o Nmap reconheceu TLS, mas não conseguiu identificar a aplicação.
- A opção `-quiet` do `openssl s_client` desativa os comandos interativos que poderiam interferir no envio da senha.
- Uma chave privada SSH pode ser salva como texto, desde que seu conteúdo e sua formatação sejam preservados.
- `ssh-keygen -y -f` permite extrair a chave pública de uma chave privada válida, sendo útil para verificar se o arquivo pode ser interpretado pelo SSH.
- Chaves privadas não devem ser publicadas em repositórios.

---

[← Nível anterior](level-15-16.md) · [Voltar ao índice](../README.md)
