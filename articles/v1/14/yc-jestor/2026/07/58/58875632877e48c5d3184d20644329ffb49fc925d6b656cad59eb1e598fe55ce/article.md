---
schema_version: "1.0.0"
document_id: "58875632877e48c5d3184d20644329ffb49fc925d6b656cad59eb1e598fe55ce"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/campo-moeda-vs-texto-jestor/"
published_at: "2026-07-22T01:58:35+00:00"
first_seen_at: "2026-07-22T02:48:39.600163+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:90002fdfde01f5d422b22ea53ea0f74704715e37d3687d0839c6cb1721b13c09"
---

# Por que um campo de moeda permite somar valores em relatórios e um campo de texto não?

No **Jestor** , um campo de moeda armazena o valor como número, permitindo que fórmulas e automações somem esses valores; um campo de texto armazena o conteúdo como uma sequência de caracteres, que não pode ser somada matematicamente.


### Por que essa diferença técnica importa


Escrever "R$ 100" em um campo de texto pode parecer visualmente igual a um campo de moeda, mas o sistema não interpreta esse conteúdo como número. Ao tentar somar valores de parcelas ou gerar relatórios financeiros, o campo de texto simplesmente não soma, gerando erro ou resultado zerado.


### O que o campo de moeda oferece a mais


- Armazena o valor como número, permitindo soma em fórmulas e automações
- Permite escolher o símbolo da moeda (real, dólar, euro, entre outros) de forma padronizada
- É compatível com leitura automática via **OCR** , preenchendo o valor extraído de uma nota fiscal
- Facilita relatórios que somam parcelas, totais de contas a pagar ou faturamento


### Como usar corretamente o campo de moeda no Jestor


1. Ao criar o campo, pesquise e selecione o tipo "moeda", não "texto" ou "número"
2. Escolha o símbolo da moeda padronizado para aquela tabela
3. Use esse campo em fórmulas que precisem somar ou calcular valores
4. Evite campos de texto para qualquer valor que precise entrar em cálculo depois


### Automação de processos financeiros com dados corretos desde o cadastro


Escolher o tipo de campo certo desde o início evita retrabalho na **automação de processos** financeiros, já que corrigir um campo de texto para moeda depois de muitos registros criados é mais trabalhoso do que configurar corretamente desde o começo.


### Resumo em tabela


Tipo de campo Pode ser somado em fórmula?


**Moeda** Sim


**Número** Sim


**Texto** Não


### Vídeo Tutorial: Passo a Passo


*Vídeo: Ep 14: Dominando Valores e Prazos — tutorial em vídeo mostrando na prática o recurso descrito neste artigo, direto na interface do **Jestor** .*


## Perguntas Frequentes


**Dá para converter um campo de texto em campo de moeda depois?** O ideal é configurar corretamente desde o início; conversões posteriores podem exigir ajuste manual dos dados já existentes.


**O campo de número também soma valores como o campo de moeda?** Sim, mas o campo de moeda tem a vantagem adicional de padronizar o símbolo da moeda exibido.


**O campo de moeda funciona com leitura automática de nota fiscal?** Sim, o **OCR** do **Jestor** pode preencher automaticamente esse campo a partir de um anexo enviado no[jestor.com](https://jestor.com/?ref=blog.jestor.com) .


## Conheça o Jestor


Com o **Jestor** , é possível automatizar fluxos, conectar áreas e criar sistemas internos do seu jeito — tudo sem código e com o suporte da **IA** . Conheça o Jestor em[jestor.com](https://jestor.com/?ref=blog.jestor.com) e descubra como levar a gestão da sua empresa a um novo nível de eficiência e integração.
