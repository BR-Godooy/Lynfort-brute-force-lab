# Contexto da análise de prevenção

A coleção contém 13 capturas de testes executados pelo autor no Kali e no Metasploitable 2, em outubro de 2026. A revisão foi documental, sem novos testes de rede ou inspeção do código do alvo.

O índice público está em [Evidências](../docs/evidencias.md). O [manifesto SHA-256](../docs/manifesto-evidencias.json) registra a origem por nome e o hash dos arquivos copiados.

## Inventário

| Captura | O que apoia |
|---|---|
| [01-kali-ip.png](../screenshots/01-kali-ip.png) | IPv4 do Kali |
| [02-kali-rede-host-only.png](../screenshots/02-kali-rede-host-only.png) | Configuração do Adaptador 1 do Kali |
| [04-reconhecimento-servicos-nmap.png](../screenshots/04-reconhecimento-servicos-nmap.png) | Serviços reportados pelo Nmap |
| [07-ftp-medusa.png](../screenshots/07-ftp-medusa.png) | Procedimento e resultado FTP |
| [08-ftp-validacao-manual.png](../screenshots/08-ftp-validacao-manual.png) | Procedimento e resultado FTP |
| [09-dvwa-seguranca-inicial-high.png](../screenshots/09-dvwa-seguranca-inicial-high.png) | Estado ou resultado do exercício web |
| [10-dvwa-seguranca-low.png](../screenshots/10-dvwa-seguranca-low.png) | Estado ou resultado do exercício web |
| [11-dvwa-tentativa-invalida.png](../screenshots/11-dvwa-tentativa-invalida.png) | Estado ou resultado do exercício web |
| [12-dvwa-medusa.png](../screenshots/12-dvwa-medusa.png) | Estado ou resultado do exercício web |
| [13-dvwa-validacao-manual.png](../screenshots/13-dvwa-validacao-manual.png) | Estado ou resultado do exercício web |
| [15-smb-enumeracao-usuarios.png](../screenshots/15-smb-enumeracao-usuarios.png) | Saída de enumeração, teste ou listagem SMB |
| [16-smb-spraying.png](../screenshots/16-smb-spraying.png) | Saída de enumeração, teste ou listagem SMB |
| [17-smb-validacao-manual.png](../screenshots/17-smb-validacao-manual.png) | Saída de enumeração, teste ou listagem SMB |

## Limites

Não há logs originais, configuração integral das VMs, código-fonte do alvo, medições de taxa, auditoria de políticas ou reteste após defesa. As listas de candidatos foram reconstruídas das imagens. Não existe scan automatizado associado à análise. Mudança de código e estado posterior do alvo: desconhecidos.
