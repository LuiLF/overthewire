# Bandit — Nível 12 → 13

## Objetivo

Recuperar a senha do próximo nível a partir de um arquivo que contém um hexdump de dados comprimidos repetidamente.

## Exploração

O arquivo `data.txt` continha uma representação hexadecimal de dados binários. Para trabalhar com os arquivos intermediários sem modificar o original, criei um diretório temporário e copiei o arquivo para ele.

```bash
mktemp -d
# /tmp/tmp.IVS9gTjW2b

cp data.txt /tmp/tmp.IVS9gTjW2b
cd /tmp/tmp.IVS9gTjW2b
```

O caminho é gerado automaticamente pelo `mktemp -d` e varia entre execuções.

Em seguida, reverti o hexdump e identifiquei o formato do arquivo resultante:

```bash
xxd -r data.txt > dados.txt
file dados.txt
```

O resultado foi identificado como um arquivo comprimido com gzip.

## Resolução

Após identificar a primeira camada, utilizei as ferramentas correspondentes para descomprimir ou extrair os dados, verificando os formatos ao longo do processo.

**Camadas gzip e bzip2:**

```bash
mv dados.txt dados.gz
gzip -d dados.gz

file dados
mv dados dados.bz2
bzip2 -d dados.bz2
```

O arquivo resultante continha outra camada gzip:

```bash
file dados
mv dados dados.gz
gzip -d dados.gz
```

**Camadas TAR:**

```bash
file dados
mv dados dados.tar
tar -x -f dados.tar

file data5.bin
mv data5.bin data5.tar
tar -x -f data5.tar

mv data6.bin data6.tar
tar -x -f data6.tar
```

Após essas extrações, identifiquei que `data8.bin` continha dados comprimidos com gzip:

```bash
file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
```

Por fim, verifiquei o formato do arquivo resultante e li seu conteúdo:

```bash
file data8
cat data8
```

O arquivo continha a senha necessária para acessar o próximo nível.

## Observações

- O `xxd -r` reconstrói os bytes originais a partir de um hexdump. O texto ASCII inicial representava dados binários, enquanto o texto ASCII final continha a senha.
- O comando `file` identifica o formato real dos dados, independentemente da extensão.
- `gzip` e `bzip2` realizam compressão de dados, enquanto `tar` permite reunir e extrair arquivos.
- Refiz o desafio para consolidar os conceitos e resolver as etapas de forma independente.

---

[← Nível anterior](level-11-12.md) · [Voltar ao índice](../README.md)
