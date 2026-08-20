---
schema_version: "1.0.0"
document_id: "73c2822a79ed5e8adaa721f584cbd95ddf433b30f4107e6dc6ac955b9c4c1263"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/pix-automatico-vs-debito-automatico-boleto-cartao"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:15:28.620730+00:00"
content_hash: "sha256:814f24aaa245f9d5361618195037ba680220cccfb573f08048c1eace607502ff"
---

# Pix Automático vs boleto vs cartão: cobrança recorrente no ERP

Todo software que cobra mensalidade ou assinatura enfrenta a mesma decisão em algum momento do roadmap: qual método de cobrança recorrente vamos oferecer pro cliente?


A resposta óbvia costuma ser "o que todos usam": boleto ou cartão. O problema é que essa resposta está desatualizada. Em janeiro de 2026, o Banco Central encerrou o débito automático interbancário via boleto entre bancos diferentes. O Pix Automático assumiu oficialmente o papel de substituto para a cobrança recorrente entre instituições distintas.


## Os quatro métodos de cobrança recorrente no ERP


-


Pix Automático — débito recorrente regulado pelo Banco Central. O cliente autoriza uma vez no app do banco e as cobranças acontecem automaticamente. De qualquer banco para qualquer banco. Liquidação D+0.


-


Débito automático tradicional — modelo antigo via CNAB, com convênio bilateral por banco. Encerrado em janeiro de 2026 para cobranças interbancárias de PJ e entidades não reguladas.


-


Boleto bancário — instrumento avulso. Para recorrência, exige novo boleto a cada ciclo, envio, monitoramento e conciliação. Não é tecnicamente "recorrente".


-


Cartão de crédito recorrente — tokenização do cartão para débito via bandeira. Liquidação em D+30, com MDR e risco de chargeback.


## 1. Custo por transação


Cartão tem o custo mais alto: MDR entre 1,5% e 3,5% por transação. Numa base de 5.000 clientes pagando R$ 500/mês, são R$ 37.500 a R$ 87.500 só em tarifa de adquirente por mês. Boleto custa de R$ 1,50 a R$ 4,00 por emissão, mais re-emissão e conciliação manual. Pix Automático fica entre R$ 0,20 e R$ 0,50 por cobrança, sem MDR percentual — menos de 0,1% do valor de uma mensalidade de R$ 500.


Veredito: Pix Automático tem o menor custo por transação.


## 2. Inadimplência involuntária


Cartão tem a maior taxa de falha involuntária: cartão expirado, limite estourado, bloqueio antifraude fazem 10% a 18% das tentativas mensais falharem sem intenção do cliente. Boleto tem inadimplência voluntária alta (15% a 25%). Pix Automático é vinculado à conta corrente, que muda com muito menos frequência que o cartão expira — a inadimplência involuntária cai para perto de zero.


O retry automático configurável trata saldo insuficiente sem intervenção humana: quando uma cobrança falha, o sistema tenta de novo em até três tentativas dentro de sete dias, nos dias e horários que o ERP configurar. Cada tentativa gera webhook com status atualizado.


## 3. Liquidação e fluxo de caixa


Cartão liquida em D+30. Boleto em D+1 a D+3. Débito automático em D+1. Pix Automático liquida em D+0: o dinheiro entra na conta no momento da execução. Para PMEs com capital de giro apertado, essa diferença é estrutural.


## 4. Cobertura de clientes


Cartão exclui quem não tem cartão ou limite — 60 milhões de brasileiros não têm cartão de crédito. Débito automático tinha cobertura limitada por convênio. Pix Automático usa a infraestrutura do Pix, que cobre mais de 900 instituições; qualquer titular de conta (CPF ou CNPJ) pode autorizar.


## 5. Chargeback e risco de fraude


Cartão é o método com maior exposição a chargeback (1% a 3% da receita bruta em setores de alta contestação). Pix Automático não tem chargeback: uma vez debitado, é definitivo. O cliente que quiser cancelar revoga o mandato — isso gera cancelamento, não estorno.


## 6. Experiência do usuário final


Cartão exige número, validade e CVV, com re-cadastro periódico. Boleto é passivo. Débito automático tinha ativação burocrática. Pix Automático tem ativação em menos de dois minutos: o cliente recebe um link, abre no app do banco que já usa, confere os dados e autoriza.


## 7. Complexidade de integração no ERP


Cartão exige gateway, tokenização e PCI DSS. Boleto exige fluxo de emissão por ciclo e conciliação por arquivo de retorno. Débito automático exigia integração por banco. Pix Automático integra via uma API REST única que cobre todos os bancos, com webhooks no lugar da conciliação manual — integração completa em uma sprint.


## Tabela resumo


-


Custo por transação — Pix Automático: baixo | Débito: médio | Boleto: médio | Cartão: alto (MDR 1,5–3,5%)


-


Inadimplência involuntária — Pix: muito baixa | Débito: baixa | Boleto: alta | Cartão: média–alta (10–18%)


-


Liquidação — Pix: D+0 | Débito: D+1 | Boleto: D+1 a D+3 | Cartão: D+30


-


Cobertura — Pix: universal (900+ inst.) | Débito: limitada | Boleto: universal | Cartão: 60M sem cartão excluídos


-


Chargeback — Pix: não | Débito: não | Boleto: não | Cartão: sim


-


Integração — Pix: baixa (API única) | Débito: alta (por banco) | Boleto: média | Cartão: alta (PCI DSS)


-


Disponível hoje — Pix: sim | Débito: encerrado (jan/2026) | Boleto: sim | Cartão: sim


## Quando boleto ou cartão ainda fazem sentido


Boleto não morreu: para cobranças avulsas e clientes que preferem pagar manualmente, segue importante e inclusivo. Cartão mantém vantagem em dois casos: quando o cliente quer parcelar (Pix Automático debita o valor integral no ciclo) e em canais internacionais, onde o Pix ainda não existe.


## Como escolher o método para o seu ERP


A escolha não precisa ser exclusiva. ERPs maduros oferecem mais de um método. Mas a ordem de prioridade é clara: Pix Automático primeiro, por cobertura, custo e liquidação. Boleto como alternativa para pagamento manual. Cartão para quem parcela ou tem preferência declarada.


Guia técnico


A Pluggy, autorizada pelo Banco Central como ITP, permite integrar Pix Automático via uma única API REST, sem licença regulatória própria. Fale com um especialista pra entender como funciona no contexto do seu produto.


[Ver changelog completo](https://www.pluggy.ai/#contato)
