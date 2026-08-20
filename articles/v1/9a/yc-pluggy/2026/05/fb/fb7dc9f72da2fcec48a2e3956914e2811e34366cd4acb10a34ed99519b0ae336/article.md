---
schema_version: "1.0.0"
document_id: "fb7dc9f72da2fcec48a2e3956914e2811e34366cd4acb10a34ed99519b0ae336"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/open-finance-2026-novidades"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:15:29.906392+00:00"
content_hash: "sha256:c6c3a5da03457dd14201f94d7a8812b1280453546f164d1f37600f607ba9a3f3"
---

# Open Finance 2026: as 4 novidades que mudam Pix, crédito e ERP no Brasil

Em 2025, o Open Finance brasileiro se tornou o maior do mundo em escala. São mais de 154 milhões de consentimentos ativos, dezenas de milhões de pessoas conectadas e um crescimento mensal que supera, em escala, o Open Banking do Reino Unido.


Tudo certo no papel, mas a prática trouxe perguntas que o sistema não estava maduro o suficiente para responder. O tesoureiro de uma empresa média ainda abre o app do banco para autorizar pagamentos. O cliente que quer portar crédito ainda assina papel na agência. O ERP ainda depende do upload de OFX para conciliar.


Em 2026, a expectativa é de mudança — não com uma virada de chave única, mas com quatro frentes que entram em produção ao longo do ano e atacam pontos de fricção concretos.


## Frente 1: Open Finance PJ ganha experiência de pessoa física


Trazer a fluidez da camada de pessoa física para o Open Finance PJ é o próximo grande salto. O ecossistema vive em duas velocidades: já superamos 1 milhão de consentimentos ativos PJ, mas o agregado nacional ainda é tímido frente ao volume da pessoa física. Enquanto o Reino Unido tem 20% das empresas conectadas, no Brasil esse número é de apenas 3%. E 99% dos consentimentos ativos no país pertencem a CPFs.


A raiz do problema é dupla. Há um desalinhamento estratégico (20% das instituições admitem que o público PJ não é prioridade) e uma fricção operacional real: hoje, se uma empresa com três sócios quer compartilhar dados, o sistema exige que os três entrem no app do banco quase ao mesmo tempo para aprovar. Na vida real, isso não acontece.


### O que o Banco Central está atacando em 2026


-


Fluxo assíncrono para múltiplos sócios (CIBA). Cada sócio aprova no seu tempo, no canal que preferir, e a operação só efetiva quando o último aprovador clica.


-


Possível revisão da Resolução BCB nº 295/2023, que hoje isenta os bancos corporates da participação obrigatória.


-


Melhoria geral do consentimento PJ, com telas mais claras sobre escopo e responsabilidades.


-


Parceria com o Sebrae para educação financeira focada em pequenos negócios.


Destravar PJ é o multiplicador de valor número um do ecossistema. Crédito B2B com análise via dados bancários reais, gestão multibancos, conciliação automatizada para tesourarias — tudo depende dessa frente. É exatamente nesse vão que a Pluggy opera, com clientes como MarketUP, Conta Azul, Nibo e Contabilizei.


## Frente 2: JSR para PJ, ou o dia em que o ERP virou o banco


A Jornada Sem Redirecionamento (JSR) virou obrigatória para todas as instituições detentoras de conta do arranjo Pix em 6 de fevereiro de 2026 (Resolução BCB nº 541/2025). Entre fevereiro e abril, a versão 2.2.0 rodou em piloto trazendo, pela primeira vez, a JSR para o segmento PJ, o ambiente desktop e o Pix Automático. Desde 22 de abril de 2026, a oferta pode chegar ao público em geral.


### O que é Jornada Sem Redirecionamento (JSR)?


JSR é a tecnologia que permite ao usuário autorizar pagamentos via Pix sem precisar sair para o app do banco. A autenticação acontece dentro do próprio ambiente onde a operação foi iniciada: o ERP, o e-commerce, o marketplace, o app do iniciador.


Antes: o tesoureiro montava um lote de 50 pagamentos no ERP, abria o app do banco, colocava senha e token, aprovava um a um e voltava pro ERP. Cada salto é um ponto de fricção. Com a JSR para PJ, o ERP é vinculado à conta da empresa como dispositivo confiável (uma única vez), e a partir daí o tesoureiro aprova com biometria ou PIN dentro do próprio ERP, sem redirecionamento, com conciliação em tempo real.


Um ponto que os documentos regulatórios não capturam: o ganho real só vem quando o vínculo de dispositivo funciona bem em primeira tentativa. Cada fricção no onboarding inicial é um cliente que volta pro fluxo antigo. Vale construir esse onboarding com mais cuidado que o necessário.


## Frente 3: portabilidade de crédito 100% digital e o que vem depois


Em fevereiro de 2026, a portabilidade de crédito 100% digital entrou em operação — a primeira aplicação transacional do Open Finance brasileiro voltada para crédito. A base legal é a Resolução Conjunta nº 15/2025 do CMN com o Banco Central. A ordem começa pelo crédito pessoal sem garantia (o caminho mais simples), e o crédito imobiliário é o destino final.


O que muda para o cliente: pedido feito direto no app da instituição proponente; prazo máximo de até 3 dias úteis no ambiente Open Finance (contra 20 a 25 dias do processo tradicional); comparação visual e transparente entre condição atual e nova oferta; e assinatura digital. Para fintechs que não são o banco principal do cliente, a portabilidade é a maior abertura de mercado em anos.


## Frente 4: jornada otimizada, o motor por trás das experiências integradas


Em abril de 2025, o Banco Central publicou a versão 7.0 do Manual de Escopo de Dados e Serviços, onde apareceu a "jornada otimizada". A implementação pelas instituições acontece ao longo de 2026.


### O que é jornada otimizada no Open Finance?


É o mecanismo que combina dois consentimentos (um primário e um secundário) em uma única aprovação do usuário, eliminando jornadas separadas para operações complementares como pagamento + leitura de saldo. Isso destrava casos como Pix Parcelado, cobrança recorrente com validação de capacidade de pagamento e análise de crédito instantânea no checkout.


O ponto de tensão: a jornada otimizada amplia o que o usuário autoriza em uma única decisão. Pesquisa da Febraban mostrou que mais da metade dos clientes que compartilham dados no Open Finance não sabem que estão fazendo isso. Juntar dois consentimentos em uma única tela só funciona se a transparência for reforçada.


## O que mais vem por aí


-


Pix Parcelado integrado ao Open Finance como vitrine de crédito.


-


Portabilidade de salário e de investimentos, ainda em estudo.


-


Open Assets, infraestrutura para circulação de recebíveis.


-


Novas regras de transferência de dados a terceiros (modelo "1x1x1").


-


Open Insurance com nova fase regulatória pela SUSEP.


2026 não é o ano em que o Open Finance "vai começar a entregar". É o ano em que ele entrega dentro do ERP, no momento do crédito, no checkout e na conciliação. Quem está construindo nessa camada agora captura a próxima onda.


Infraestrutura pronta


A Pluggy é ITP autorizada pelo Banco Central, conecta a 99%+ dos bancos brasileiros via uma única API e processa mais de 1 milhão de conexões por mês. As quatro frentes deste artigo já estão sendo construídas na nossa infraestrutura.


[Ver changelog completo](https://www.pluggy.ai/#contato)
