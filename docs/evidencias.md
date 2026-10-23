# Evidências do laboratório

[Voltar ao projeto](../README.md)

## Capturas selecionadas

A entrega contém **13 imagens**: 12 capturas de ambiente e testes selecionadas da pasta recebida, mais uma captura da rede do Kali. Os arquivos foram copiados sem edição dos pixels.

| Cenário | Captura | O que permite conferir |
|---|---|---|
| Ambiente | [01-kali-ip.png](../screenshots/01-kali-ip.png) | Interface eth0 com 192.168.56.102/24. |
| Ambiente | [02-kali-rede-host-only.png](../screenshots/02-kali-rede-host-only.png) | Adaptador 1 em Host-only; demais adaptadores e VM alvo não estão documentados nesta imagem. |
| Reconhecimento | [04-reconhecimento-servicos-nmap.png](../screenshots/04-reconhecimento-servicos-nmap.png) | Comando, destino e cinco portas abertas, com identificação de serviços. |
| FTP | [07-ftp-medusa.png](../screenshots/07-ftp-medusa.png) | Quatro combinações; sucesso reportado para msfadmin/msfadmin. |
| FTP | [08-ftp-validacao-manual.png](../screenshots/08-ftp-validacao-manual.png) | 230 Login successful e consulta do diretório remoto. |
| DVWA | [09-dvwa-seguranca-inicial-high.png](../screenshots/09-dvwa-seguranca-inicial-high.png) | Configuração anterior; sem teste de resistência nesse nível. |
| DVWA | [10-dvwa-seguranca-low.png](../screenshots/10-dvwa-seguranca-low.png) | Confirmação do nível Low e PHPIDS desabilitado. |
| DVWA | [11-dvwa-tentativa-invalida.png](../screenshots/11-dvwa-tentativa-invalida.png) | Mensagem de falha para a senha fictícia incorreta. |
| DVWA | [12-dvwa-medusa.png](../screenshots/12-dvwa-medusa.png) | Duas tentativas; sucesso reportado para admin/password. |
| DVWA | [13-dvwa-validacao-manual.png](../screenshots/13-dvwa-validacao-manual.png) | Mensagem de boas-vindas à área protegida do exercício. |
| SMB | [15-smb-enumeracao-usuarios.png](../screenshots/15-smb-enumeracao-usuarios.png) | Lista retornada pelo enum4linux; comando inicial fora do recorte. |
| SMB | [16-smb-spraying.png](../screenshots/16-smb-spraying.png) | Uma senha para user, service e msfadmin; sucesso reportado para msfadmin. |
| SMB | [17-smb-validacao-manual.png](../screenshots/17-smb-validacao-manual.png) | smbclient -L retorna compartilhamentos; não comprova acesso a seu conteúdo. |

## Como ler os resultados

O FTP tem confirmação explícita de login e consulta do diretório. No DVWA, a resposta de boas-vindas confirma a combinação dentro do exercício, com sessão existente e nível Low. No SMB, a ferramenta reporta sucesso e o cliente lista serviços com o usuário informado; não foram avaliadas todas as permissões nem excluído todo possível mapeamento convidado.

Senhas padrão e valores fictícios foram usados deliberadamente. Não medimos resistência em escala, ausência de bloqueio, eficácia de MFA ou controles após endurecimento. As versões são as identificações reportadas nas imagens.

## Organização dos nomes

A numeração original foi preservada quando possível, com lacunas. A captura da rede recebeu o número 02. A associação a seguir registra os arquivos de origem por nome, sem publicar caminhos do computador do autor.

| Nome de origem | Nome nesta entrega |
|---|---|
| 01-Kali-ip.png | 01-kali-ip.png |
| Captura da configuração do Adaptador 1 do Kali | 02-kali-rede-host-only.png |
| 04-reconhecimento-servicos-nmap.png | 04-reconhecimento-servicos-nmap.png |
| 07-ftp-medusa.png | 07-ftp-medusa.png |
| 08-ftp-validacao-manual.png | 08-ftp-validacao-manual.png |
| 09-dvwa-segurnaca-inicial-high.png | 09-dvwa-seguranca-inicial-high.png |
| 10-dvwa-segurnaca-inicial-low.png | 10-dvwa-seguranca-low.png |
| 11-dvwa-tentatica-invalidada.png | 11-dvwa-tentativa-invalida.png |
| 12-dvwa-medusa.png | 12-dvwa-medusa.png |
| 13-dvwa-validacao-manual.png | 13-dvwa-validacao-manual.png |
| 15-smb-enumeracao-usuarios.png | 15-smb-enumeracao-usuarios.png |
| 16-smb-spraying.png | 16-smb-spraying.png |
| 17-smb-validacao-manual.png | 17-smb-validacao-manual.png |

As capturas efetivamente fornecidas usam **12 para o terminal DVWA e 13 para o navegador**. Por isso, esta entrega segue os arquivos reais, em lugar da sequência planejada durante a conversa.

## Integridade e seleção

O [manifesto SHA-256](manifesto-evidencias.json) contém o tamanho e o hash de cada imagem. Ele permite verificar se a cópia desta entrega foi alterada; não é uma assinatura independente sobre a execução dos testes. Os hashes das cópias foram conferidos contra os arquivos recebidos.

O rascunho de preparação das listas e as fotos de diagnóstico ficaram fora do procedimento final. A tese original foi preservada na pasta de trabalho, fora deste pacote público. O [artigo revisado](fundamentos-autenticacao.md) incorpora seus conceitos com referências.

Nenhum valor literal de cookie de sessão foi identificado nas imagens selecionadas. O comando web contém apenas a variável local. Há IPs privados, MAC, nomes locais e datas do laboratório. Não foram copiados histórico de shell, cabeçalhos HTTP ou HTML de diagnóstico.

## Listas e registros

As quatro listas de [wordlists](../lab/wordlists/) foram reconstruídas dos valores visíveis nas tentativas finais. Os caminhos e os nomes estão organizados para esta entrega e identificados nos roteiros.

**Não há logs originais de terminal nesta pasta.** Os pequenos trechos textuais dos roteiros são transcrições identificadas das capturas, não registros originais reconstruídos. As figuras são a base preservada para conferir entradas e respostas.

