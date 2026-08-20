---
schema_version: "1.0.0"
document_id: "1e1872d5a08882363f1b8d46382f0f752fb1ae4ed31dbf69035770c08eeaa88b"
company_key: "yc-stark-bank"
company: "STARK BANK"
source_id: "yc-stark-bank-rss-c29e4cd527eb"
canonical_url: "https://blog.starkbank.com/split-de-pagamento/"
published_at: "2026-08-13T13:47:00+00:00"
first_seen_at: "2026-08-13T16:13:33.131186+00:00"
fetched_at: "2026-08-13T16:13:35.040674+00:00"
content_hash: "sha256:bd8bad838ddc056722813bd75c2623752adfdd8435b2bd98cd190e2978d51f62"
---

# Split de pagamento: como automatizar a liquidação em marketplaces e e-commerces sem bitributação

*Entenda como o split de pagamento automatiza a divisão de vendas em marketplaces e e-commerces. Evite a bitributação e integre sua API em tempo real*


Para uma plataforma que vende por meio de vários lojistas ao mesmo tempo, o split de pagamento é essencial para crescer com segurança e sem risco fiscal. Afinal, cada venda que entra precisa ser dividida entre marketplace, vendedores e parceiros, e a forma como essa divisão acontece muda o quanto cada um paga de imposto.


Neste artigo, explicamos o que é o split de pagamento e como ele funciona na prática, por que ele é a principal defesa contra a bitributação, o que o Banco Central exige das plataformas que movimentam dinheiro de terceiros. Boa leitura!


## **Afinal, o que é o split de pagamento e por que ele é essencial para e-commerces e marketplaces?**


O split de pagamento é o mecanismo que divide, de forma automática, o valor de uma única transação entre múltiplos recebedores. Em uma venda de R$ 1.000 no marketplace, por exemplo, R$ 850 podem ir para o vendedor, R$ 120 para a plataforma (o chamado take rate) e R$ 30 para um parceiro logístico, tudo definido por regras programadas, sem intervenção humana.


Esse mecanismo deixou de ser um recurso de nicho para virar infraestrutura essencial do varejo digital. Os marketplaces concentram cerca de 70% das compras online do Brasil, ou seja, a maior parte do dinheiro do comércio eletrônico brasileiro passa, hoje, por alguma forma de divisão de valores entre plataforma e vendedores. Para entender o conceito desde a base, veja[como funciona o split de pagamento](https://blog.starkbank.com/como-funciona-o-split-de-pagamento) .


### **Como funciona o modelo split de pagamento na prática de liquidação**


Do clique do comprador ao saldo na conta do vendedor, o fluxo de um modelo split de pagamento bem desenhado percorre quatro etapas. Primeiro, o comprador paga um carrinho unificado, que pode reunir produtos de vários vendedores. Em seguida, o[gateway de pagamento](https://blog.starkbank.com/gateway-de-pagamento) captura e processa a cobrança. A partir daí:


- O motor de regras do split identifica quem participa daquela venda e aplica as divisões configuradas: percentuais, valores fixos ou uma combinação dos dois;
- Os valores líquidos são provisionados diretamente para cada destino: a comissão na conta do marketplace e o valor de venda na conta de cada vendedor;
- Cada movimentação gera um registro próprio, rastreável no extrato, que alimenta a contabilidade das partes de forma independente.


O ponto central é este: a divisão acontece na liquidação, e não depois dela. O dinheiro já chega separado, e é exatamente isso que resolve o problema tributário que veremos a seguir.


### **Quem usa split além dos marketplaces?**


Embora o marketplace seja o exemplo clássico, o split de pagamento sustenta qualquer negócio em que uma venda tem mais de um dono. Um exemplo são plataformas SaaS que cobram pelos seus lojistas, como sistemas de agendamento, delivery ou educação, que usam o split para reter a mensalidade ou o percentual da transação e repassar o restante ao cliente final. Em todos esses casos, a lógica é a mesma: quanto mais cedo o dinheiro se separa, menos trabalho manual, menos risco fiscal e menos discussão sobre quem deve o quê para quem.


## **Como evitar a bitributação em marketplaces usando split de pagamento**


Aqui mora a dor mais cara do modelo antigo. Quando o valor total da venda entra na conta corrente do marketplace para depois ser repassado manualmente ao vendedor, o Fisco tende a interpretar que a plataforma faturou o montante inteiro. Resultado: tributos como ISS ou ICMS podem incidir sobre um dinheiro que nunca foi receita da empresa – a chamada bitributação, que corrói a margem de qualquer take rate.


Já com o split de pagamento na liquidação bancária, essa distorção desaparece. Como cada participante recebe apenas a sua fatia diretamente na própria conta, cada um emite nota fiscal estritamente sobre o seu ganho real: o marketplace sobre a comissão, o vendedor sobre o valor do produto. Desta forma, a estrutura fiscal reflete a realidade econômica da operação e a empresa deixa de pagar imposto sobre o dinheiro dos outros.


Para visualizar o tamanho do problema, basta fazer a conta: em uma plataforma que intermedia R$ 10 milhões por mês com take rate de 12%, a receita real é de R$ 1,2 milhão. Se o Fisco entender que o faturamento foi de R$ 10 milhões, a base de cálculo dos tributos é multiplicada por mais de oito, sobre um dinheiro que apenas passou pela conta. No entanto, com a divisão na origem, nada disso existe.


Além do custo direto, há o risco: uma autuação sobre anos de repasses mal estruturados pode consumir de uma vez a margem que a operação levou anos para construir. Por isso, CFOs de plataformas de alto volume tratam o desenho do fluxo de liquidação como decisão estratégica, não como detalhe operacional.


### **O papel regulatório do Banco Central e das contas de liquidação**


Não é só o Fisco que olha para esse fluxo. No âmbito do SPB (Sistema de Pagamentos Brasileiro), o Banco Central regula como as ordens de pagamento são liquidadas e exige que plataformas que movimentam recursos de terceiros operem com instituições homologadas. Na prática, isso significa que o marketplace não deve “segurar” dinheiro de vendedores em conta própria: os valores precisam transitar por uma estrutura regulada, como a de um[Provedor de Serviços de Pagamento (PSP)](https://blog.starkbank.com/o-que-e-psp-e-como-funciona) ou de uma instituição de pagamento autorizada.


Essa exigência protege todas as partes: o vendedor, que não fica exposto à insolvência da plataforma; o comprador, que tem a transação processada dentro do sistema financeiro regulado; e o próprio marketplace, que evita penalidades administrativas e questionamentos regulatórios. Ou seja, split é tanto engenharia quanto compliance.


## **Como fazer split no Pix: a nova era das transações instantâneas em lote**


Se o split já era importante no cartão, no Pix ele virou urgência. O Pix se tornou o meio de pagamento líder do e-commerce brasileiro, com 42% do valor transacionado em 2025, à frente do cartão de crédito, segundo o Global Payments Report da Worldpay. E o Pix tem uma característica que muda tudo: o dinheiro chega em segundos e o vendedor espera que a sua parte também chegue na mesma velocidade.


Por isso, o split no Pix exige duas coisas da tecnologia financeira: velocidade na divisão e notificação síncrona. No momento em que o Pix é recebido, o sistema aplica as regras de divisão e credita as contas de destino imediatamente, enquanto um webhook notifica a plataforma e o vendedor sobre cada centavo movimentado. Com uma[API de Pix para empresas](https://blog.starkbank.com/pix-api) , esse fluxo roda de ponta a ponta sem lote, sem fila e sem espera.


O impacto operacional é direto: o vendedor vê o saldo disponível, despacha a mercadoria sem atraso e mantém o próprio fluxo de caixa saudável, o que, para o marketplace, se traduz em vendedores mais satisfeitos, SLA de entrega menor e menos atrito na ponta.


## **Conciliação tradicional vs. reconciliação instantânea: qual a diferença?**


O modelo legado de split, oferecido por adquirentes tradicionais, costuma funcionar em lote: os repasses são processados em D+1 ou D+2, misturados em relatórios extensos que a equipe financeira precisa destrinchar planilha por planilha. Em operações de alta volumetria transacional, isso significa fechamento diário sempre atrasado e erros descobertos dias depois de acontecerem.


O Stark Bank resolve esse gargalo com[conciliação bancária automatizada](https://blog.starkbank.com/conciliacao-bancaria) integrada via API: cada movimentação de split gera um registro síncrono e rastreável no extrato digital, no exato momento em que acontece. A comparação lado a lado mostra a diferença:


**Diferencial operacional** **Sistemas de split tradicionais** **Modelo do Stark Bank**


Velocidade de liquidação Processamento manual em lote (D+1 ou D+2), gerando atrito com os sellers Processamento em tempo real direto na conta, inclusive para split de Pix de alto volume


Contas e regras de repasse Limitações para abertura de subcontas e dependência de integrações complexas Criação instantânea de recebedores e regras de split por API, com painel transparente


Estabilidade e SLA de API APIs antigas e lentas, com falhas em picos de vendas Infraestrutura bancária própria, com alto SLA e documentação REST completa


Gestão contábil Sistemas terceiros para conciliar relatórios e repasses de comissões Conciliação síncrona, associada automaticamente ao extrato financeiro da empresa


### **Webhooks e ERP: como integrar o split à sua arquitetura**


Para o time de engenharia, a integração é direta. A criação de recebedores (as subcontas de vendedores e parceiros) acontece dinamicamente por API, com payloads estruturados: no onboarding de um novo lojista, a plataforma cria a conta de destino e as regras de comissionamento, fixas ou percentuais, na mesma chamada.


Já a saída de dados roda por webhooks: cada evento de liquidação dispara uma notificação que o ERP consome para registrar a operação contábil automaticamente. Com resiliência de entrega e reprocessamento em caso de falha, nenhum evento se perde: o extrato, o ERP e o painel contam sempre a mesma história.


## **Como configurar o split de pagamento no painel financeiro do Stark Bank**


No Stark Bank, a robustez da tecnologia de API convive com a simplicidade de um painel financeiro moderno. A configuração parte de três blocos: criação dos recebedores, definição das regras de divisão e ativação dos webhooks de notificação. Do lado do dia a dia, o painel consolida o fluxo de caixa da operação inteira, com cada split registrado de forma transparente no extrato.


Nossos clientes contam com atendimento rápido e eficiente e uma API REST desenhada para times de tecnologia que precisam escalar a eficiência operacional sem depender de intermediários. Se a sua operação está avaliando a infraestrutura financeira como um todo, vale ler também sobre o[melhor banco para e-commerce de alto volume](https://blog.starkbank.com/banco-para-ecommerce) .


Conheça o[Stark Bank](https://starkbank.com/) e descubra como implementar o split de pagamento com liquidação em tempo real na sua empresa: entre em contato e veja a tecnologia funcionando na prática.


## **Perguntas frequentes sobre split de pagamento**


### **O que é o split de pagamento e qual a sua principal vantagem?**


O split de pagamento é a tecnologia que divide automaticamente o valor de uma venda online entre diferentes partes, como o marketplace e o vendedor final. A principal vantagem é evitar a bitributação: como cada participante recebe apenas o que lhe cabe, cada um é tributado somente sobre o próprio ganho real, protegendo a margem do negócio.


### **Como fazer split no Pix de forma segura?**


O split no Pix deve ser feito por meio de uma API de pagamento conectada de forma síncrona a uma instituição financeira regulada. No momento do recebimento do Pix, o sistema identifica as regras de divisão configuradas e executa, de forma automática e imediata, o crédito nas contas de destino dos recebedores.


### **Como o split de pagamento impacta a conciliação bancária?**


Em sistemas legados, o split dificulta a conciliação, porque os repasses chegam misturados em relatórios descentralizados. Já com uma API de alta performance, como a do Stark Bank, a reconciliação é síncrona: cada repasse registra transferências e tarifas de modo transparente, diretamente no extrato financeiro do painel.
