# Notas sobre Verificação de Identidade Digital

## O problema
O clamor pela transformação digital e a pressão por serviços remotos nos períodos pandêmico e pós-pandêmico são motores de inovação e exigem criteriosos requisitos de segurança e privacidade.

Qualquer serviço digital que permita o registro ou criação de contas (_onboarding_) está passível de fraude por parte do usuário solicitante, variando de contas _fake_ (onde o usuário não corresponde a uma pessoa legítima) a impersonificação (onde um agente malicioso se passa por uma pessoa legítima).

Devido à dificuldade e ao alto custo muitos serviços abrem mão da verificação, assumindo os riscos de negócio e de imagem. Porém, a alternativa nem sempre pode ir à mesa devido à regulações, _compliance_ ou necessidades de verificação rígidas associadas a custo, requisitos de negócio e privacidade. Redes sociais são exemplos de indústrias com baixo foco em verificação (e alta poluição com perfis _fakes_), em oposição às _fintechs_ (com seus processos de _onboarding_ 100% remotos).

Projetos governamentais frequentemente fazem esse tipo de análise de _trade-off_. Isso parece estar mudando, evidências dadas pelas iniciativas federal ([gov.br](https://acesso.gov.br/)), estadual ([LoginSP](https://login.sp.gov.br/)) e municipal ([Login Único](https://legislacao.prefeitura.sp.gov.br/leis/decreto-60663-de-25-de-outubro-de-2021/detalhe)) (entre tantas outras) de centralização de identidade do cidadão.

No Brasil esse processo é reconhecidamente complexo devido à inexistência de cadastros compreensivos unificados com um nível de completude capaz de atender a diferentes casos de uso, e à miríade de documentos de identificação em suas múltiplas versões.

Nesse cenário, serão discutidas alternativas para a implantação da verificação de identidade em um cenário plausível de uma aplicação governamental para prestação de serviços ao cidadão, onde ao se registrar é requerida a genuína verificação da identidade do próprio usuário, bem como sua presença física junto ao dispositivo no ato do cadastro.

_Verificação de identidade_, neste texto, se refere a combinação de diversas atividades realizadas durante o evento de interação onde o usuário alega possuir uma identidade no mundo real, geralmente em sua primeira interação (registro, cadastro, _onboarding_ e afins). Em diversos níveis de tolerância, busca-se verificar se a alegação procede, confirmando se a identidade real existe, se o usuário alegando posse da identidade é o seu verdadeiro possuidor, e se sua presença durante o processo é genuína.

Além da verificação, outros métodos podem ser utilizados, com menor nível de confiabilidade, para adicionar evidências de suporte à verificação. Vamos chamá-los de métodos de _afirmação de identidade_. Seu uso como única fonte de verificação é desencorajado.

Buscaremos alternativas à tradicional (e altamente confiável) verificação presencial de identidade realizada por agentes públicos em balcões de atendimento físicos, como no caso do serviço de identidade paulistano [Senha Web](https://www.prefeitura.sp.gov.br/cidade/secretarias/fazenda/servicos/senhaweb/), em prol de soluções nativamente remotas em canais digitais.

## Técnicas
Várias alternativas são disponibilizadas pelo mercado. As soluções mais robustas devem utilizar a combinação de diversas técnicas para alcançar um nível de confiabilidade adequado ao problema.

Abaixo, algumas das alternativas.

### Verificação de identidade do mundo real com foco em documentos
Consiste no uso de dispositivos de captura de imagem (_webcams_ ou câmeras de _smartphones_) em canais digitais remotos (tipicamente aplicações baseadas em _browser_ ou aplicativos _mobile_) para obtenção de imagens ou vídeos de documentos físicos.

![](img/doc_verify.png)  
[Captura de documento em aplicação _mobile_ (Imagem: biometricupdate.com)](https://www.biometricupdate.com/201905/digital-identity-and-document-verification-market-to-generate-15-billion-by-2024)

A partir da captura podemos:

- identificar o documento (é um documento conhecido? [é um RG](https://www.terra.com.br/noticias/infograficos/nova-carteira-de-identidade/index.htm) ou uma CNH?);
- buscar por sinais de adulteração e falsificação;
- realizar OCR dos textos;
- obter a foto de identificação.

Normalmente esse processo está associado a um mecanismo de detecção de presença "ao vivo" (_Liveness Detection_, ou _Presentation Attack Detection_). Nesse caso, a foto do documento é comparada com uma _selfie_  do usuário (imagem ou vídeo) tirada no durante o processo. Caso haja uma base prévia de imagens confiável, também é possível realizar o reconhecimento facial.

![](img/selfie.png)  
[Reconhecimento facial de _selfie_ (Imagem: computerid.com.br)](https://computerid.com.br/solucoes/solucoes_view.php?c=10&s=16&p=9)

Este método entrega um nível aceitável de confiança, dadas as verificações de "algo que _somente você possui_" (seu documento) e de "algo que _somente você é_" (sua face).

A técnica também é conhecida como "ID+_selfie_" ou "eKYC" (_eletronic- Know Your Consumer_).

### Afirmação de identidade do mundo real com foco em dados
Consiste na comparação dos dados fornecidos pelo usuário (nome, data de nascimento, endereço, …) com bases de dados confiáveis (registros governamentais, dados censitários, dados financeiros, …).

Pode se manifestar como uma verificação passiva (os dados são fornecidos pelo usuário e verificados pela aplicação) ou por algum método de desafio-resposta (onde o usuário é desafiado a responder corretamente a alguma pergunta criada com base em seus dados conhecidos previamente pela aplicação).

![](img/questions.png)  
[Questionário de confirmação de identidade em gov.br](http://faq-login-unico.servicos.gov.br/en/latest/_perguntasdafaq/contaacesso.html#cadastro-com-as-informacoes-basicas-do-cidadao)

Esta técnica foi a mais utilizada pelo mercado durante muito tempo. Porém, entrega um nível baixo de confiança dada a verificação única de "algo que _não somente você sabe_" (seus dados pessoais).

Considerando o crescimento de vazamentos de dados, engenharia social e _malwares_, não é recomendado seu uso como verificação de identidade, mas somente como método de afirmação.

### Afirmação de identidade com foco em dispositivos
Consiste na geração de um identificador único do usuário baseado em informações combinadas de hardware e software do dispositivo do usuário, chamado de _device fingerprint_.

![](img/device-fingerprint.png)  
[Identificação do usuário mesmo em abas anônimas (fingerprintjs.com)](https://fingerprintjs.com/)

Pode ser utilizado para inferir risco em alguns casos de usos, quando utilizado em acessos subsequentes. Por exemplo, várias contas sendo criadas pelo mesmo dispositivo podem ser consideradas indício de fraude, assim como o acesso pelo mesmo dispositivo onde o cadastro foi criado pode indicar um sinal de confiança.

### Afirmação de identidade com foco em atributos digitais
Consiste no uso de atributos digitais (como e-mail, endereços IP e perfis em redes sociais) para afirmação de identidade, preferencialmente em correlação com identidades do mundo real.

Segundo o Gartner, o e-mail tem se provado um atributo de identidade particularmente persistente tendendo a permanecer por grandes períodos sem alteração.

Apesar de não poderem ser utilizados como identificação única e inequívoca, dados de geolocalização por IP podem ajudar a identificar fraudes caso divirjam consideravelmente do histórico do usuário ou de endereços previamente conhecidos.

![](img/ip-geolocation.png)  
[Exemplo de dados obtidos via serviço de geolocalização por IP](https://www.ip2location.com/demo/)

Também é comum considerar e-mails ou perfis recém-criados ou não encontrados em nenhuma base de dados como potencialmente arriscados.

### Afirmação de identidade com foco em análise de comportamento
Consiste na criação de perfil de usuário com base em seu comportamento (cadência de digitação, padrão de movimento de ponteiro do mouse, padrão de toque/arrasto), geralmente no primeiro uso de uma aplicação (ou de outra aplicação que compartilhe o perfil).

Pode ser usado para obter indícios de confiança na identidade ou de fraude. Por exemplo, usuários podem ter dificuldades no preenchimento de um formulário que um agente malicioso não apresentaria (por repetir muitas vezes, ou por ser um _bot_ pré-programado).

### Afirmação de identidade com foco em número de telefone
Consiste no uso de dados obtidos pela rede telefônica para correlacionar o usuário com uma identidade do mundo real.

Entre as possibilidades de uso estão:

- Ligação feita pelo dispositivo a uma central que identifica a chamada;
- Análise de sinal para detecção de possíveis grampos;
- Correlação de número telefônico com cadastro de proprietário;
- Idade do _SIM card_ (se muito recente pode indicar risco de fraude);
- Criação de perfil de voz;
- Identificação por voz (em acessos subsequentes);
- _Login_ baseado em verificação do dispositivo (por exemplo, via GSMA Mobile Connect).

![](img/gsma-mobile-connect.png)  
[Brasil listado entre os países que suportam _GSMA Mobile Connect_](https://www.gsma.com/identity/mobile-connect-deployment-map)

## BYOI - _Bring Your Own Identity_
Em busca de reduzir a quantidade de identidades digitais que uma pessoa gerencia, o conceito de BYOI traz a ideia de reutilização das suas identidades reconhecidas em uma fonte confiável em outros pontos de acesso.

A maioria das implementações usam _Single Sign-On_ (SSO) via [OAuth](https://oauth.net/) e [OpenID Connect](https://openid.net/). O nível de confiabilidade esta diretamente correlacionada com a confiança no emissor.

![](img/oauth-providers.png)  
[Exemplos de serviços online que permitem login social no Auth0](https://auth0.com/blog/social-login-on-the-rise/)

Em termos de UX, a escolha de alguns poucos provedores é preferida. A disponibilidade de muitas opções prejudica a experiência do usuário tanto visualmente (["NASCAR problem"](https://indieweb.org/NASCAR_problem)) quanto funcionalmente (["paradoxo da escolha"](https://en.wikipedia.org/wiki/The_Paradox_of_Choice)).

### Redes Sociais
Identidades gerenciadas por serviços _online_, como redes sociais e e-mails (Facebook, Google, Microsoft, Apple, …).

![](img/social-login.png)  
[Tela de _login_ do Slack com opções de _login_ social](https://slack.com/signin#/signin)

Podem garantir a diferenciação entre um usuário e outro, mas quase nada podem fazer para garantir que o usuário é quem ele diz ser (por exemplo, o possuidor de um CPF específico). O grande número de contas _fake_ torna desencorajador o seu uso como único método de identificação de uma pessoa real (apesar de frequentemente ser suficiente para o comércio de produtos e serviços, por exemplo, onde a realização segura do pagamento - que independe da identidade - é o objetivo final).

Comumente são utilizadas somente para autenticação, em forma de vínculos adicionais a uma conta já verificada anteriormente.

### Governo
Identidades digitais atestadas por uma entidade governamental responsável, geralmente carregando o peso das verificações físicas associadas.

O [gov.br](https://acesso.gov.br/) e o [LoginSP](https://login.sp.gov.br/) são iniciativas com esse fim, e permitem a integração das identidades entre diversos sistemas governamentais com alta confiabilidade e suporte a SSO com OAuth/OpenID. Em São Paulo, o [Senha Web](https://www.prefeitura.sp.gov.br/cidade/secretarias/fazenda/servicos/senhaweb/) possui verificação física de identidade em balcões de atendimento.

![](img/login-ecpf.png)  
[Senha Web permite autenticação com certificado digital](https://senhawebsts.prefeitura.sp.gov.br/)

Um exemplo híbrido é o [e-CPF](https://certificadodigital.imprensaoficial.com.br/certificados-digitais/e-cpf) (digital ou digital+físico, como nos tokens A3) que inclui as entidades certificadoras no processo, permitindo adicionalmente serviços como assinatura digital de documentos.

![](img/token-ecpf-a3-prodesp.png)  
[e-CPF físico (A3) emitido pela Prodesp](https://certificadodigital.imprensaoficial.com.br/certificados-digitais/e-cpf)

É utilizado, por exemplo, para afirmação de identidade no [gov.br](http://faq-login-unico.servicos.gov.br/en/latest/_perguntasdafaq/comoadquirircertificadodigitalpessoafisica.html), e como método de autenticação no [Senha Web](https://www.prefeitura.sp.gov.br/cidade/secretarias/fazenda/servicos/senhaweb/).

![](img/certi-digital-afirmacao.png)  
[Afirmação de identidade com certificado digital no gov.br](http://faq-login-unico.servicos.gov.br/en/latest/_perguntasdafaq/comoadquirircertificadodigitalpessoafisica.html#como-atribuir-o-selo-certificado-digital-de-pessoa-fisica)

O uso de documentos legíveis por máquina através de QRCode e NFC apresenta um notável potencial de expansão, pois já é suportado por alguns documentos como a CNH, passaportes e o novo RG brasileiro.

### Instituições Financeiras
Bancos possuem uma grande base de usuários verificados (inclusive por razões de _compliance_) e podem prestar o serviço de verificação de identidade.

Com o crescimento do padrão [Open Banking](https://openbankingbrasil.org.br/) isso se torna uma realidade factível e de simples implementação, tornando os bancos em potenciais provedores de identidade para SSO.

No processo de registro do gov.br dois métodos são disponibilizados em parceria com instituições bancárias. Em um deles usa-se SSO, e no outro gera-se um código de verificação (código NAI do _cidadão.br_/DataPrev) no _internet banking_ que é validado no processo de registro.

![](img/bancos-sso.png)  
[SSO com instituições bancárias no gov.br](https://acesso.gov.br/)

![](img/first-access-nai.png)  
[Uso do NAI no registro do cidadão.br](https://mte.api.dataprev.gov.br/auth/login?pat_first_access=true)

### Operadoras de Telefonia
O padrão [GSMA Mobile Connect](https://www.gsma.com/identity/mobile-connect) permite que operadoras de telefonia disponibilizem serviços de identidade com alta portabilidade, vinculados aos _SIM cards_ dos dispositivos.

A autenticação e a afirmação são aplicações práticas, porém a confiabilidade da verificação varia de acordo com os requisitos nacionais de identificação no registro de compra de _SIM cards_, além de possuir cobertura limitada a poucos países (o Brasil é um deles).

Sua operação permite o SSO aprovado automaticamente quando solicitado pelo próprio dispositivo, ou então a confirmação via SMS caso em outro dispositivo.

![](img/mobile-connect.png)  
[Exemplo de implementação de Mobile Connect com WSO2](https://wso2.com/library/webinars/2016/11/securing-access-to-saas-apps-with-gsma-mobile-connect/)

No futuro deve incluir biometria nos cadastros, o que pode simplificar o processo e melhorar a confiabilidade.

### Uso Corporativo e Provedores de BYOI
O mercado corporativo de verificação de identidade é bastante variado, incluindo provedores de identidade com as mais diversas fontes de verificação.

![](img/idme-students.png)  
[O provedor ID.me possui _compliance_ para diversos serviços governamentais americanos. Imagem: Lenovo.com](https://www.id.me/business)

Além disso, organizações costumam integrar seus próprios diretórios de colaboradores como fontes de identidade, normalmente usando LDAP e Active Directory.

![](img/sso-ad.png)  
[Acesso usando conta Azure AD integrada para alunos das Escolas Técnicas Estaduais](http://etec.sp.gov.br/)

### Identidade sem senha (_passwordless_)
A especificação [WebAuthn](https://webauthn.guide/) e o projeto [FIDO2](https://fidoalliance.org/fido2/) abrem portas para um modelo de identidade desvinculado de senhas, integrando com autenticadores fortes nos dispositivos, como o [Windows Hello] e o [Apple Touch ID].

Nesse modelo são criadas duas chaves de criptografia no momento do registro: uma pública e uma privada. A chave privada é armazenada de forma segura no dispositivo do usuário. A pública é enviada para ser armazenada pelo servidor, e e não tem valor nenhum para um agente malicioso. Na autenticação, o servidor envia dados para que sejam assinados digitalmente pelo usuário, usando a sua chave privada. Ao receber os dados assinados e verificar com a chave pública armazenada, pode-se garantir que eles vieram do usuário correto.

Essa especificação já está implementada nos navegadores mais modernos e pode permitir uma autenticação super segura envolvendo a biometria do usuário armazenada em seu dispositivo, bem como o uso de autenticadores biométricos físicos.

## Considerações para aquisição
### Viés demográfico
As soluções de detecção facial são probabilísticas e dependem dos seus algoritmos e dos seus dados de treinamento. Há [diversos](https://arxiv.org/pdf/2103.01592.pdf) [estudos](http://proceedings.mlr.press/v81/buolamwini18a/buolamwini18a.pdf) que mostram diferença significativa nos resultados de acordo com características demográficas como sexo, idade e etnia.

![](img/demo-bias-gendershades.png)  
[Algoritmos possuem menor precisão ao avaliar mulheres negras, segundo o estudo do GenderShades](http://gendershades.org/)

Dois tipos de erros são mais comuns:

- Falso positivo: duas imagens de pessoas diferentes são avaliadas como a mesma pessoa;
- Falso negativo: duas imagens da mesma pessoa são avaliadas como pessoas diferentes.

Segundo o Gartner, o número de falsos positivos entre pessoas asiáticas pode chegar a ser 100 vezes maior do que entre caucasianos. Esse número é reduzido em algoritmos desenvolvidos na Ásia. Falsos negativos são mais comuns em mulheres e pessoas jovens.

É importante avaliar os resultados e medir os riscos de negócio, legais e de imagem.

### Corroboração de identidade
A afirmação baseada em dados normalmente se baseia em dados de fontes autoritativas convencionais, como fontes governamentais, financeiras, postais ou eleitorais.

O uso de fontes não convencionais, como verificação em entidades parceiras, pode melhorar os resultados. Por exemplo, diferentes _e-commerces_ em uma mesma rede de confiança poderiam verificar se um usuário consta em uma base compartilhada, reduzindo a possibilidade de fraude.

Dados de registro também podem ser comparados em bases de dados de vazamentos, que poderiam indicar um possível uso indevido.

### Orquestração
Quanto maior o rol de funcionalidades que o caso de uso exija, menor será a chance de encontrar um fornecedor que as ofereça em um único produto. O grande desafio que se apresenta é realizar a orquestração dos diversos provedores em um _workflow_ robusto que não degrade a experiência do usuário, aumente a complexidade da solução, ou os custos. Um único ponto de integração é o ideal.

São características de um orquestrador:

- Configuração de _workflows_ (registro, autenticação, recuperação, …), preferencialmente em estilo _no-code_;
- Integração entre diversos fornecedores de verificação e afirmação;
- Normalização dos diferentes formatos de dados obtidos;
- Gestão de políticas de _workflow_, de forma a controlar a UX (_go_/_no-go_);
- Ferramentas de análise e monitoramento;
- Flexibilidade para execução de testes A/B, facilitando a mudança de políticas;
- Redundância de _workflows_ entre diferentes fornecedores.

![](img/workflow-okta.gif)  
[Interface de configuração de _workflow_ de identidade do Okta](https://www.okta.com/platform/workflows/workflows-for-lifecycle-management/)

## _Features_ de provedores de identidade
### Integração
Suporte a aplicações web e _mobile_, disponibilização de SDKs e APIs, _deploys_ _on premises_ ou _cloud_.

### Captura de imagens de documentos
Disponibilização de interface amigável de captura.

### Cobertura de formatos
Identificação e OCR de diferentes documentos de diversas geografias, e seu processo de manutenção contínua.

### _Selfie_ e _liveness detection_
Interface para captura ao vivo de _selfie_, permitindo verificação da presença do usuário.

### NFC, códigos de barras e QRCodes
Suporte à extração de dados previamente armazenados em diferentes mídias.

### Automação
Uso de processos automatizados na maioria dos casos, usando validação por analistas somente em casos extremos.

### Acurácia
Baixa presença de falsos positivos/negativos e alta detecção de fraudes.

### Gestão do viés demográfico
Existência de métricas e clareza no trato da questão.

### Armazenamento de dados de identidade
Clareza e controle do cliente sobre os métodos e locais de armazenamento dos dados que identificam um usuário.

### Conexões com terceiros
Disponibilidade de integrações para verificação e afirmação pré-construídas, e facilidade na configuração/construção de novas.

### Deduplicação
Detecção e tratamento de duplicação de identidades.

### Autenticação
Configuração de diferentes fluxos para registro, autenticação e recuperação de contas de acordo com os requisitos do negócio.

### Aumento de dados de perfil
Dados que o provedor é capaz de adicionar a um perfil criado, por exemplo usando dados públicos ou parcerias de integração.

### _Dashboards_ e relatórios
Ferramentas de gestão do processo.

## Fornecedores

Abaixo, uma lista **não extensiva** de fornecedores de serviços de verificação identidade. Foram listados somente _players_ em que foi possível encontrar alguma referência de atendimento ao Brasil.

- [Acuant](https://www.acuant.com/) *
- [Daon](https://www.daon.com/) *
- [Serasa Experian](https://www.serasaexperian.com.br/solucoes/crosscore/) *
- [Idemia](https://www.idemia.com/) *
- [Idwall](https://idwall.co/)
- [Jumio](https://www.jumio.com/) *
- [Trulioo](https://www.trulioo.com/) *
- [Unico](https://unico.io/unico-check/)
- [4stop](https://4stop.com/) *

\* constantes em listagens do Gartner

## Referências
- KHAN, Akif; CARE, Jonathan. Market Guide for Identity Proofing and Affirmation. Gartner, 2020.
- KHAN, Akif; CARE, Jonathan. Buyer’s Guide for Identity Proofing. Gartner, 2021.
- TERHORST, Philipp; KOLF, Jan N; _et al_. [A Comprehensive Study on Face Recognition Biases Beyond Demographics](https://arxiv.org/pdf/2103.01592.pdf). Journal of Latex class files, vol. 14, no. 8, 2015.
- BUOLAMWINI, Joy; GEBRU, Timnit. [Gender Shades: Intersectional Accuracy Disparities in
Commercial Gender Classification](http://proceedings.mlr.press/v81/buolamwini18a/buolamwini18a.pdf). Proceedings of Machine Learning Research 81:1–15, 2018.

![](img/qr-pdf.png)  
© _Prodam/SP - Núcleo de Inovação_, Março de 2022.