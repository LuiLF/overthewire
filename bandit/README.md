# Bandit — OverTheWire

Documentação dos meus estudos no [Bandit](https://overthewire.org/wargames/bandit/), wargame introdutório do OverTheWire voltado à familiarização com o terminal Linux e seus principais comandos.

O objetivo é registrar os conceitos explorados, as ferramentas utilizadas e os aprendizados adquiridos durante a resolução dos desafios.

## Ambiente

- **Plataforma:** OverTheWire
- **Wargame:** Bandit
- **Acesso:** SSH pelo PowerShell (Windows 10)
- **Sistema:** Ambiente Linux remoto disponibilizado pela plataforma

## Progresso

| Nível | Conceitos explorados | Status |
|---|---|---|
| [0](./levels/level-00.md) | Conexão remota via SSH | Concluído |
| [0 → 1](./levels/level-00-01.md) | Listagem e leitura de arquivos | Concluído |
| [1 → 2](./levels/level-01-02.md) | Leitura de arquivos com nomes especiais | Concluído |
| [2 → 3](./levels/level-02-03.md) | Leitura de arquivos com espaços e hífens no nome | Concluído |
| [3 → 4](./levels/level-03-04.md) | Localização e leitura de arquivos ocultos | Concluído |
| [4 → 5](./levels/level-04-05.md) | Identificação de tipos de arquivo com `file` | Concluído |
| [5 → 6](./levels/level-05-06.md) | Busca de arquivos por tamanho com `find` | Concluído |
| [6 → 7](./levels/level-06-07.md) | Busca de arquivos por proprietário, grupo e tamanho com `find` | Concluído |
| [7 → 8](./levels/level-07-08.md) | Busca de padrões em arquivos com `grep` | Concluído |
| [8 → 9](./levels/level-08-09.md) | Identificação de linhas únicas com `sort` e `uniq` | Concluído |
| [9 → 10](./levels/level-09-10.md) | Extração de texto de arquivos binários com `strings` e filtragem com `grep` | Concluído |
| [10 → 11](./levels/level-10-11.md) | Codificação e decodificação de dados com Base64 | Concluído |
| [11 → 12](./levels/level-11-12.md) | Substituição de caracteres e decodificação ROT13 com `tr` | Concluído |
| [12 → 13](./levels/level-12-13.md) | Hexdump (`xxd`), identificação de arquivos (`file`), gzip, bzip2 e TAR | Concluído |
| [13 → 14](./levels/level-13-14.md) | SSH com chave privada (`ssh -i`), transferência de arquivos (`scp`) e diagnóstico de conexões | Concluído |
| [14 → 15](./levels/level-14-15.md) | Netcat (`nc`), conexões TCP, localhost e portas de rede | Concluído |
| [15 → 16](./levels/level-15-16.md) | OpenSSL (`s_client`), handshake TLS, certificados digitais e conexões criptografadas | Concluído |
| [16 → 17](./levels/level-16-17.md) | Nmap (`-sV`), identificação de serviços TCP/TLS, OpenSSL e autenticação SSH por chave privada | Concluído |
| [17 → 18](./levels/level-17-18.md) | Comparação de arquivos com `diff` e identificação de linhas alteradas | Concluído |
| [18 → 19](./levels/level-18-19.md) | SSH com execução remota de comandos, `.bashrc` e diferenciação entre maiúsculas e minúsculas | Concluído |
| [19 → 20](./levels/level-19-20.md) | SetUID, execução de binários ELF, permissões e acesso a arquivos protegidos | Concluído |
| [20 → 21](./levels/level-20-21.md) | Netcat, comunicação TCP, cliente-servidor, tmux e controle de processos | Concluído |
| [21 → 22](./levels/level-21-22.md) | Cron, tarefas agendadas, scripts Bash, permissões e redirecionamento de saída | Concluído |
| [22 → 23](./levels/level-22-23.md) | Cron, scripts Bash, variáveis, pipes, MD5, cut e contexto de execução | Concluído |
| [23 → 24](./levels/level-23-24.md) | Cron, scripts Bash, permissões, stat, condicionais, timeout e redirecionamento de saída | Concluído |

---

[← Voltar ao OverTheWire](../README.md)
