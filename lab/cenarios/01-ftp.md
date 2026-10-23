# FTP e autenticação por lista de candidatos

[Voltar ao projeto](../../README.md) · [Próximo cenário — DVWA](02-dvwa.md)

## Objetivo e condições

Verificar se o FTP do Metasploitable aceita uma credencial padrão presente em uma lista pequena. O alvo é `192.168.56.103`, porta 21, identificado como vsftpd 2.3.4. A origem é o Kali.

Duas contas e duas senhas produzem quatro combinações possíveis. A credencial `msfadmin / msfadmin` foi inserida como controle conhecido do ambiente, documentado pela [Rapid7](https://docs.rapid7.com/metasploit/metasploitable-2/).

## Entradas utilizadas

As tentativas finais mostram estas entradas, reconstruídas nos arquivos da entrega:

| [ftp-usuarios.txt](../wordlists/ftp-usuarios.txt) | [ftp-senhas.txt](../wordlists/ftp-senhas.txt) |
|---|---|
| `usuarios_teste` | `senhas_incorreta123` |
| `msfadmin` | `msfadmin` |

A grafia no plural corresponde à execução final da captura.

## Procedimento

Na VM, as listas se chamavam `usuarios.txt` e `senhas.txt`. O comando registrado foi:

```bash
medusa -h 192.168.56.103 -U usuarios.txt -P senhas.txt -M ftp -t1 -F -O 07-medusa-listas-corrigidas.txt
```

Para repetir a partir da raiz desta entrega, os caminhos foram ajustados:

```bash
medusa -h 192.168.56.103 \
  -U lab/wordlists/ftp-usuarios.txt \
  -P lab/wordlists/ftp-senhas.txt \
  -M ftp -t 1 -F -O ftp-reexecucao.txt
```

`-U` e `-P` recebem arquivos; `-t 1` usa uma autenticação por vez; `-F` encerra ao encontrar uma credencial válida; `-O` acrescenta registros. Use um arquivo novo para separar execuções. Consulte a [ajuda do Medusa](https://www.kali.org/tools/medusa/).

O comando adaptado não foi reexecutado durante a preparação desta entrega.

## Resultado observado

![Quatro combinações testadas e sucesso para msfadmin](../../screenshots/07-ftp-medusa.png)

As quatro tentativas estão visíveis. A ferramenta reporta:

```text
User: msfadmin Password: msfadmin [SUCCESS]
```

Esse trecho é uma transcrição parcial da imagem, não um arquivo original de log.

## Confirmação manual

Foi utilizado o cliente FTP:

```bash
ftp 192.168.56.103
```

Após informar `msfadmin / msfadmin`, foi executado `pwd`.

![Login FTP confirmado e diretório remoto retornado](../../screenshots/08-ftp-validacao-manual.png)

A evidência mostra `230 Login successful.` e `Remote directory: /home/msfadmin`. Isso comprova autenticação aceita e consulta ao diretório atual; não demonstra transferência de arquivos.

O encerramento com `bye` foi relatado durante o laboratório. A captura preservada termina antes dele.

## Leitura defensiva

A aceitação da credencial padrão sustenta a recomendação de substituí-la e revisar a necessidade do serviço. Quatro tentativas não comprovam ausência de qualquer bloqueio ou limitação de taxa.

Se for necessária transferência de arquivos em infraestrutura mantida, podemos avaliar SFTP sobre SSH, gestão de chaves e permissões mínimas. O transporte criptografado não impede que uma senha previsível seja aceita. Veja os critérios de reteste em [Prevenção](../../hardening/hardening.md).
