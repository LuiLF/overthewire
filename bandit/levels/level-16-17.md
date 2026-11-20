# Bandit — Nível 16 → 17

## Objetivo

Identificar os serviços disponíveis nas portas TCP de `31000` a `32000` do `localhost`, distinguir quais utilizam SSL/TLS e localizar o serviço que fornece as credenciais do próximo nível mediante o envio da senha atual.

## Exploração

Inicialmente, utilizei o Nmap para identificar as portas abertas no intervalo informado:

```bash
nmap localhost -p 31000-32000
```

O scan identificou cinco portas abertas, mas não reconheceu os serviços em execução.

Como já conhecia a opção `-sV` de exercícios anteriores no TryHackMe, realizei uma nova varredura com detecção de serviços:

```bash
nmap -sV localhost -p 31000-32000
```

O resultado foi:

```text
PORT      STATE SERVICE
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

A identificação revelou dois serviços que utilizavam TLS.

Inicialmente, escolhi a porta `31518`, identificada como `ssl/echo`, por apresentar um serviço reconhecido pelo Nmap.

Ao estabelecer a conexão com `openssl s_client`, percebi que a senha começava com `k`, acionando o comando interativo `KEYUPDATE` em vez de transmitir o conteúdo normalmente.

Tentei novamente utilizando `-quiet`, mas a sessão foi encerrada sem fornecer as credenciais esperadas.

Após analisar o comportamento dos serviços, identifiquei que `echo` apenas devolve os dados recebidos e que `unknown` indica um serviço não reconhecido, não necessariamente indisponível.

## Resolução

Estabeleci uma conexão TLS com a outra porta identificada:

```bash
openssl s_client -quiet -connect 127.0.0.1:31790
```

Enviei a senha atual e o servidor respondeu com:

```text
Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
[conteúdo omitido]
-----END OPENSSH PRIVATE KEY-----
```

Diferentemente dos níveis anteriores, o serviço forneceu uma chave privada SSH, em vez de uma senha convencional.

## Observações

- O `nmap` identifica portas abertas, enquanto a opção `-sV` realiza sondagens adicionais para identificar serviços e versões.
- Uma porta aberta não revela necessariamente qual aplicação está em execução.
- `echo` identifica um serviço que devolve os dados recebidos; `ssl/echo` utiliza TLS para essa comunicação.
- `ssl/unknown` indica que o Nmap reconheceu TLS, mas não conseguiu classificar a aplicação.
- As respostas coletadas durante a detecção de serviços podem fornecer pistas mesmo quando o serviço permanece desconhecido.
- A opção `-quiet` do `openssl s_client` desativa os comandos interativos que poderiam interferir no envio de dados.
- Chaves privadas SSH devem ser protegidas com permissões restritas e não devem ser publicadas em repositórios.

---

[← Nível anterior](level-15-16.md) · [Voltar ao índice](../README.md)
