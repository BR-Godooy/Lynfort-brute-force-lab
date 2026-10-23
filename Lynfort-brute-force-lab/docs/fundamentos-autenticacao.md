# Autenticação e ataques a senhas: da teoria ao laboratório

**Victor Godoy · Lynfort · Artigo de estudo**

Este artigo desenvolve as ideias do meu rascunho produzido durante a trilha da DIO e as relaciona aos testes documentados neste projeto. A ideia central permanece: firewall e antivírus precisam ser acompanhados de controle de acesso e configuração adequada. Um serviço disponível, com uma credencial padrão aceita, representa um risco que merece atenção própria.

O objetivo foi entender o que acontece entre informar uma senha e obter acesso, distinguir diferentes ataques e avaliar quais conclusões os experimentos realmente permitem. Os conceitos abaixo também incluem assuntos estudados que não foram executados no laboratório.

[Voltar à apresentação do projeto](../README.md)

## O que acontece depois de digitar uma senha

Autenticação verifica o controle de um autenticador associado à identidade declarada. Informar um nome de usuário não comprova essa identidade: é necessário validar uma senha, uma chave ou outro autenticador. A autorização decide quais recursos e ações aquela identidade pode utilizar. Estar autenticado não significa ter permissão para tudo. Essa distinção aparece nas orientações da [OWASP sobre autenticação](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) e [autorização](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

Em um exemplo fictício, uma pessoa pode entrar no portal de uma empresa e consultar seus documentos sem poder administrar as contas dos colegas. Validar o login e conferir a permissão para cada documento são decisões diferentes.

A sessão mantém a continuidade dessas interações. Em um modelo comum, o servidor relaciona um identificador aos dados da sessão e o navegador envia esse identificador em um cookie. O navegador guarda o cookie; o estado da sessão pode permanecer no servidor. Depois do login, outras requisições continuam exigindo validação da sessão e das permissões. Consulte a [OWASP — Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

## Stateful, stateless e federação

Em uma arquitetura **stateful**, o servidor mantém informações de sessão que ajudam a reconhecer a continuidade do acesso. Em uma abordagem **stateless**, cada requisição fornece o necessário para sua validação, sem depender daquele estado de sessão no servidor. Isso não significa ausência de banco de usuários, configuração ou mecanismos de revogação.

Nenhum dos dois nomes garante segurança. Expiração, proteção do transporte e validação correta continuam necessárias. Tokens também exigem verificação de integridade, emissor e destinatário, conforme o mecanismo adotado. A arquitetura precisa ser avaliada pelo funcionamento real, e não somente pela presença de cookies ou tokens. Veja a [OWASP — REST Security](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html).

**HTTP Basic** é um esquema específico, não um sinônimo de toda autenticação stateless. Ele representa usuário e senha em Base64. Essa codificação é reversível e não fornece criptografia. O transporte precisa ser protegido, normalmente por HTTPS/TLS. A [RFC 7617](https://www.rfc-editor.org/rfc/rfc7617.html) explicita essa limitação.

Na **autenticação federada**, uma aplicação confia no resultado de autenticação emitido por um provedor de identidade. OpenID Connect, por exemplo, acrescenta uma camada de identidade ao OAuth 2.0. A aplicação precisa validar a resposta recebida, incluindo emissor, destinatário e validade, conforme a [especificação OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Minha interpretação é que centralizar a identidade pode facilitar políticas consistentes, mas também cria dependências. Federação não é automaticamente superior: configuração, recuperação de conta, confiança no provedor e proteção da integração precisam fazer parte da análise.

## Diferentes maneiras de tentar uma senha

O termo força bruta costuma aparecer como um nome amplo para tentativas repetidas. Em sentido estrito, uma busca exaustiva percorre todas as combinações de um espaço definido. Um ataque de dicionário seleciona candidatos de uma lista; um híbrido aplica transformações a esses candidatos. A organização das tentativas também diferencia spraying e stuffing.

| Técnica | Como os candidatos são escolhidos | Exemplo conceitual fictício |
|---|---|---|
| Busca exaustiva | Combinações de um conjunto e comprimento definidos | Todas as combinações possíveis de um pequeno código de teste |
| Dicionário | Entradas previamente selecionadas | Palavras de uma lista educativa |
| Híbrido | Palavras com transformações ou combinações | Palavra fictícia acrescida de um sufixo previsível |
| Password spraying | Uma ou poucas senhas distribuídas entre muitas contas | A mesma senha de teste para diferentes usuários |
| Credential stuffing | Pares de usuário e senha obtidos em outro comprometimento | Reutilização de um par vazado em outro serviço |

Spraying não garante que o tráfego será tratado como normal. A distribuição entre contas muda o padrão que a defesa precisa reconhecer. Stuffing, por sua vez, explora a reutilização de credenciais já conhecidas. As distinções são descritas pela [OWASP — Credential Stuffing Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html) e pela [MITRE ATT&CK — Brute Force](https://attack.mitre.org/techniques/T1110/).

A viabilidade depende do espaço de candidatos, da previsibilidade das senhas, do custo de cada tentativa e dos controles do serviço. Ferramentas não tornam qualquer senha viável de descobrir, nem o resultado depende apenas da velocidade do computador.

## Tentativas online e offline

No teste **online**, as tentativas chegam ao serviço que verifica as credenciais. O servidor pode registrar falhas, limitar o ritmo ou exigir fatores adicionais. Foi essa a modalidade dos três cenários deste projeto.

No ataque **offline**, o processamento ocorre sobre material obtido anteriormente, como hashes de senhas. A limitação de login do site não controla essas comparações locais. A [MITRE diferencia essas modalidades](https://attack.mitre.org/techniques/T1110/).

Armazenar senhas com algoritmos apropriados de derivação, salt e custo configurado dificulta ataques contra uma base comprometida. Hashing de senha tem função diferente da criptografia do transporte: ambos precisam ser considerados. A [OWASP — Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) explica as escolhas de armazenamento. Não houve obtenção de hashes nem cracking offline neste laboratório.

## Como reduzir o risco

Uma política de senhas deve favorecer senhas longas e únicas, permitir gerenciadores e impedir valores comuns ou comprometidos. O NIST orienta não exigir trocas periódicas arbitrárias, mas exigir a troca diante de evidência de comprometimento. Acrescentar exigências de símbolos sem considerar previsibilidade não resolve o problema. Referência: [NIST SP 800-63B-4, seção sobre senhas](https://pages.nist.gov/800-63-4/sp800-63b.html#passwordver).

MFA combina fatores distintos; duas senhas continuam sendo o mesmo tipo de fator. Quando disponível, a preferência deve ser por autenticação resistente a phishing, como implementações adequadas de FIDO/WebAuthn. Códigos digitados manualmente não oferecem essa mesma resistência. Veja o [NIST sobre resistência a phishing](https://pages.nist.gov/800-63-4/sp800-63b.html#phishres).

A integração precisa respeitar o protocolo. Acrescentar MFA a um portal não protege automaticamente o acesso direto por FTP ou SMB. Minha recomendação para uma arquitetura real é verificar todos os caminhos de autenticação, retirar serviços dispensáveis e avaliar mecanismos compatíveis com cada acesso. Neste laboratório, MFA não foi configurado nem testado.

A limitação de tentativas deve considerar a conta, evitando depender apenas do IP de origem. Bloqueios precisam ser desenhados para não facilitar a indisponibilidade de contas legítimas. CAPTCHA pode ser uma camada complementar, sem substituir os demais controles. Consulte a [OWASP — proteção contra ataques automatizados](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#protect-against-automated-attacks).

Também é necessário observar padrões agregados: várias contas recebendo tentativas semelhantes, origens distribuídas e falhas persistentes em diferentes intervalos. Logs e alertas precisam evitar o registro de senhas e tokens. A [OWASP discute monitoramento e limites de bloqueios por IP](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html#defense-in-depth-metrics).

**Cookies e tokens CSRF têm funções específicas.** Um token CSRF ajuda a impedir que outro site induza o navegador autenticado a enviar uma ação indesejada. Ele não é um limitador de tentativas de senha. A simples presença de um cookie tampouco comprova proteção contra automação. Veja a [OWASP — CSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).

Proteger a sessão inclui transporte seguro, atributos apropriados de cookie, expiração e renovação do identificador nos momentos adequados. Roubar ou reutilizar uma sessão é um problema distinto de adivinhar a senha. A [OWASP — Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html) detalha esses cuidados.

## O que observei no laboratório

No [cenário FTP](../lab/cenarios/01-ftp.md), uma lista pequena continha a credencial padrão conhecida `msfadmin` / `msfadmin`. O Medusa reportou sucesso e o acesso foi confirmado manualmente. Isso demonstra uma credencial aceita naquele ambiente; não demonstra descoberta de uma senha complexa por busca exaustiva.

No [cenário Web](../lab/cenarios/02-dvwa.md), o teste ocorreu no exercício **Brute Force do DVWA v1.0.7, nível Low**, usando uma sessão já autenticada. A combinação `admin` / `password` constava como controle conhecido na lista. A mensagem de boas-vindas confirmou o resultado no formulário. O teste não representa quebra do login principal do DVWA nem do nível High.

No [cenário SMB](../lab/cenarios/03-smb.md), foram enumerados usuários e selecionadas três contas: `user`, `service` e `msfadmin`. Uma única senha, `msfadmin`, foi testada para essas contas. A ferramenta reportou sucesso para `msfadmin`, seguido de uma verificação manual da listagem de compartilhamentos. É uma demonstração reduzida da lógica de spraying, sem medição de eficácia em escala.

As credenciais padrão foram fornecidas intencionalmente. Os testes não avaliaram credential stuffing, MFA ou resistência após aplicar bloqueios. Minhas [recomendações defensivas](../hardening/hardening.md) permanecem propostas para uma etapa posterior. O aprendizado foi relacionar cada resultado ao serviço, à configuração e à evidência correspondente, sem tratar uma mensagem da ferramenta como conclusão suficiente.

---

**Artigo elaborado a partir do estudo de Victor Godoy, com revisão técnica e editorial apoiada por IA. Referências consultadas em 23 de outubro de 2026.**
