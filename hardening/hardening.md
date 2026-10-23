# Prevenção e validação futura

[Voltar ao projeto](../README.md) · [Índice de evidências](../docs/evidencias.md)

As capturas sustentam medidas concretas sobre credenciais, exposição e validação de resultados. Minha recomendação é começar por controles locais verificáveis em uma cópia de estudo, preservando o alvo vulnerável original. Não precisamos propor uma nova arquitetura de identidade para explicar os resultados deste laboratório.

## Base de evidências

Esta análise documental foi preparada com apoio do Codex Security e revisão das imagens fornecidas. Não é resultado de um scan automatizado nem de inspeção do código das VMs.

O [login FTP](../screenshots/08-ftp-validacao-manual.png) comprova autenticação e consulta do diretório. O [formulário DVWA](../screenshots/13-dvwa-validacao-manual.png) confirma a entrada no exercício Low com sessão previamente autenticada. O [spraying SMB](../screenshots/16-smb-spraying.png) reporta uma credencial válida; a [verificação com smbclient](../screenshots/17-smb-validacao-manual.png) mostra uma listagem, sem demonstrar leitura, escrita ou privilégios administrativos.

O [Adaptador 1 do Kali](../screenshots/02-kali-rede-host-only.png) está em Host-only. A configuração completa das VMs e do hospedeiro não foi fornecida. As credenciais válidas eram conhecidas e foram fornecidas deliberadamente como entradas dos testes. Não medimos bloqueio, MFA, resistência prolongada ou desempenho.

O inventário está em [context.md](context.md), com identidade da coleção registrada em [hardening.json](hardening.json) e hashes das capturas no [manifesto](../docs/manifesto-evidencias.json).

## Restrições

O objetivo é um estudo reproduzível, de escopo pequeno, em VMs próprias. Metasploitable e DVWA antigos foram escolhidos por sua finalidade didática. Não são uma base para implantação de serviços reais.

Para evoluir, podemos clonar o ambiente ou construir um serviço equivalente em plataforma suportada. Isso preserva a comparação e evita confundir uma mudança de versão com a eficácia isolada de um controle. As instruções da documentação atual de Samba ou de servidores web precisam ser conferidas na versão que receberá a alteração.

Não há orçamento de desempenho, memória ou disponibilidade informado. Nenhuma projeção abaixo representa medição.

## Medidas proporcionais às evidências

A análise prioriza correções e configuração por serviço. As evidências mostram pontos concretos de autenticação e exposição; ainda não conhecemos a configuração completa que permitiria avaliar uma mudança de arquitetura.

Podemos relacionar cada medida a um resultado e definir como uma rodada futura demonstraria sua aplicação:

| Prioridade e medida proposta | Evidência que motiva | Critério de validação futura | Custo ou limite |
|---|---|---|---|
| **1. Substituir credenciais padrão** | FTP, Web e SMB aceitam as combinações conhecidas | No clone, as combinações anteriores falham; a credencial nova e autorizada funciona; dependências são verificadas | Trocar um segredo sem inventário pode interromper serviços |
| **2. Restringir o alcance dos serviços** | O Kali alcança cinco portas do alvo | Documentar todos os adaptadores e rotas; em sistema mantido, testar uma origem permitida e outra negada dentro do escopo autorizado | Uma origem permitida ainda pode usar uma senha fraca; regras podem afetar integrações |
| **3. Proteger o transporte** | FTP aceita login; DVWA utiliza HTTP e senha em GET | Em serviço substituto suportado, SFTP funciona e FTP dispensável é desativado; web usa HTTPS e não coloca senhas em URL ou logs | Migração de clientes, certificados e chaves; POST não substitui TLS |
| **4. Limitar tentativas por conta e observar o conjunto** | Listas automatizam tentativas; spraying percorre três usuários | Definir janela, limite e recuperação antes do ensaio; verificar contador por conta e alertas entre contas; não gravar senhas para correlacionar eventos | Bloqueio rígido pode negar serviço ao usuário legítimo; correlação exige retenção e ajuste |
| **5. Exigir fator adicional na fronteira adequada** | Uma senha conhecida permite a entrada no exercício web | Em aplicação ou IdP compatível, a senha correta sozinha não conclui o acesso; testar recuperação e revogação | Disponibilidade e recuperação do segundo fator; não presumir MFA nativo em FTP ou SMB |
| **6. Reduzir enumeração e permissões SMB** | Usuários e compartilhamentos foram listados | Consulta sem identidade autorizada deixa de enumerar quando desnecessária; contas de teste acessam apenas recursos permitidos; validar leitura e escrita separadamente | Remover recursos antigos pode quebrar clientes; assinatura SMB não substitui senhas fortes |
| **7. Manter versões e revisar protocolos** | Banners e rodapé identificam software antigo | Em infraestrutura mantida, inventariar versão suportada e compatibilidade, validar clientes antes de retirar protocolos legados | Não aplicar configuração atual cegamente ao Samba antigo do alvo didático |

## Recomendação

Recomendo priorizar credenciais e alcance dos serviços. O efeito esperado é simples de verificar e está diretamente ligado ao que foi observado: uma combinação padrão deixa de ser aceita e um cliente fora do segmento permitido deixa de alcançar o serviço. Nenhuma dessas mudanças substitui as demais. Uma senha única ainda precisa de transporte protegido; uma restrição de rede ainda permite tráfego de clientes autorizados.

No formulário web, podemos combinar limitação por conta, análise de comportamento e MFA quando suportado. Um bloqueio permanente após poucas falhas pode virar negação de serviço contra uma conta legítima. Atrasos progressivos ou uma decisão adaptativa exigem estado e monitoramento; CAPTCHA é um complemento e não uma prova de resistência. As recomendações de bloqueio e monitoramento estão na [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html).

Senhas longas, únicas e verificadas contra listas de senhas comuns ou comprometidas são preferíveis a padrões previsíveis. O [NIST SP 800-63B-4](https://pages.nist.gov/800-63-4/sp800-63b.html) orienta não exigir troca periódica arbitrária e exigir mudança diante de evidência de comprometimento. Uma futura recuperação de conta deve preservar essas garantias.

Para transferência de arquivos, o [SFTP do OpenSSH](https://man.openbsd.org/sftp.1) utiliza transporte SSH criptografado. Isso protege o canal, mas não impede o uso de uma credencial válida conhecida. Da mesma forma, HTTPS não elimina a exposição de uma senha na URL, que pode permanecer em históricos e registros. Veja a [orientação da OWASP sobre query strings](https://community.owasp.org/vulnerabilities/Information_exposure_through_query_strings_in_url).

No SMB, podemos revisar identidade convidada, enumeração e permissões em conjunto, utilizando a [documentação de configuração do Samba](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html) apropriada ao servidor suportado de destino. A [documentação do smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html) delimita o que a listagem demonstra. O teste não prova ausência de ACL nem deve ser descrito como controle administrativo do sistema.

Essas mudanças têm efeitos operacionais: contadores e correlação consomem estado e armazenamento; segundo fator adiciona dependência e fricção; migração de protocolo exige novos clientes. Não foram medidas latência, memória ou disponibilidade. A decisão deve considerar o fluxo legítimo, sua carga habitual e como recuperar o acesso caso a mudança falhe.

## Próximas decisões

Uma rodada futura deve partir de um clone e registrar o estado anterior, o controle alterado, o caso positivo legítimo e o caso que deve ser rejeitado. Mudamos um controle por vez quando isso for necessário para atribuir o resultado.

Antes do ensaio, precisamos definir políticas de tentativa e recuperação, contas de teste, origens permitidas e negadas, plataforma suportada e compatibilidade dos clientes. Também devemos decidir se o objetivo é fortalecer o serviço antigo para estudo ou migrar a função para outro serviço. As comparações respondem a perguntas diferentes.

A conclusão só muda de **proposto** para **validado** quando essa execução for registrada. As recomendações desta pasta permanecem não implementadas no laboratório entregue.
