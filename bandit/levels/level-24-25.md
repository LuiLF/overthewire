# Bandit 24 → 25

## Objetivo

Obter a senha do usuário `bandit25` por meio de um serviço TCP na porta `30002`, fornecendo a senha atual do `bandit24` e um PIN numérico de quatro dígitos.

O PIN deveria ser descoberto por força bruta, testando as 10.000 combinações possíveis, de `0000` a `9999`.

## Investigação

O enunciado informava que não era necessário estabelecer uma nova conexão para cada tentativa.

A estratégia inicial foi utilizar um loop para gerar os PINs e o `nc` (Netcat) para enviá-los ao serviço em `localhost:30002`.

Durante a implementação, foi necessário compreender como o Bash trabalha com loops, variáveis, formatação numérica e pipelines.

Também foi considerada a diferença entre abrir uma conexão TCP para cada tentativa e reutilizar uma única conexão para transmitir todas as combinações.

## Resolução

### 1. Geração dos PINs

Foi utilizado um loop aritmético em Bash para percorrer os valores de `0` a `9999`.

```bash
for ((i=0; i<10000; i++)); do
    pin=$(printf "%04d" "$i")
    echo "SENHA_DO_BANDIT24 $pin"
done
```

O comando `printf "%04d"` garante que cada PIN tenha quatro dígitos, incluindo zeros à esquerda.

A substituição de comando `$(...)` permite armazenar o resultado do `printf` na variável `pin`.

### 2. Envio das tentativas

Para evitar a abertura de milhares de conexões, a saída do loop foi direcionada para uma única execução do Netcat:

```bash
for ((i=0; i<10000; i++)); do
    pin=$(printf "%04d" "$i")
    echo "SENHA_DO_BANDIT24 $pin"
done | nc localhost 30002
```

O serviço recebia cada tentativa no formato:

```text
SENHA_DO_BANDIT24 PIN
```

O pipe (`|`) após o `done` permite encaminhar todas as linhas produzidas pelo loop para a entrada padrão do `nc`, mantendo uma única conexão TCP.

### 3. Resultado

Após executar o script com a senha do `bandit24`, o serviço processou as combinações enviadas e identificou o PIN correto.

```text
Correct!
The password of user bandit25 is [SENHA]
```

A senha foi recuperada com sucesso, concluindo o nível.

## Observações técnicas

- O Netcat permite enviar dados para um serviço TCP pela entrada padrão (`stdin`).
- Colocar `nc` dentro do loop executaria o programa repetidamente, criando novas conexões.
- Colocar o pipe após `done` permite enviar a saída de todo o loop para uma única execução do `nc`.
- A formatação `%04d` é necessária para preservar os zeros à esquerda dos PINs.
- O comando `$(...)` captura a saída de outro comando e permite armazená-la em uma variável.
- A abertura repetida de conexões pode gerar carga desnecessária em um serviço, mesmo quando não há intenção de causar indisponibilidade.

## Conceitos aprendidos

- Automação de força bruta em ambiente autorizado.
- Loops aritméticos com `for` em Bash.
- Formatação numérica com `printf`.
- Variáveis e substituição de comandos.
- Comunicação TCP utilizando Netcat.
- Redirecionamento de dados com pipelines.
- Reutilização de conexões TCP.
---

[← Nível anterior](level-23-24.md) · [Voltar ao índice](../README.md)
