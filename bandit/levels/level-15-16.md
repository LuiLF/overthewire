# Bandit — Nível 15 → 16

## Objetivo

Obter a senha do próximo nível enviando a senha atual para um serviço disponível na porta `30001` do `localhost`, utilizando uma conexão criptografada com SSL/TLS.

## Exploração

Analisei as ferramentas sugeridas pelo desafio e identifiquei que o `nmap` não seria necessário, pois a porta de destino já estava informada.

Para estabelecer uma conexão criptografada, utilizei o OpenSSL com o subcomando `s_client`.

Na primeira tentativa, executei:

```bash
openssl s_client -connect 127.0.0.1 30001
```

O comando retornou um erro de sintaxe, pois a opção `-connect` espera o endereço e a porta no formato `host:porta`.

Após consultar a mensagem de erro, corrigi a sintaxe e estabeleci a conexão.

## Resolução

Executei o comando:

```bash
openssl s_client -connect 127.0.0.1:30001
```

O OpenSSL realizou o handshake TLS e exibiu informações sobre o certificado, o protocolo e a criptografia negociada.

Entre os resultados apresentados:

```text
CONNECTED(00000003)
Protocol: TLSv1.3
Cipher: TLS_AES_256_GCM_SHA384
Verify return code: 18 (self-signed certificate)
```

Após o estabelecimento da conexão, enviei a senha atual pelo terminal.

O serviço respondeu com `Correct!` e retornou a senha do próximo nível, concluindo o desafio.

## Observações

- `openssl s_client` permite estabelecer conexões TLS e inspecionar informações da negociação criptográfica.
- A opção `-connect` utiliza o formato `host:porta`.
- TLS é o sucessor do SSL, que atualmente é considerado obsoleto.
- O handshake TLS negocia os parâmetros necessários para proteger a comunicação.
- O servidor apresentou um certificado autoassinado. Embora a conexão tenha sido criptografada com TLS 1.3, a identidade do servidor não foi validada por uma autoridade certificadora confiável.
- Diferentemente do nível anterior, que utilizava TCP sem TLS, este desafio exigia uma comunicação criptografada.

---

[← Nível anterior](level-14-15.md) · [Voltar ao índice](../README.md)
