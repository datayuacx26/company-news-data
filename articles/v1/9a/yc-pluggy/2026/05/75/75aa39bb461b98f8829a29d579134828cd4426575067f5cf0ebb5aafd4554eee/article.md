---
schema_version: "1.0.0"
document_id: "75aa39bb461b98f8829a29d579134828cd4426575067f5cf0ebb5aafd4554eee"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/pix-automatico-como-funciona"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:15:28.620730+00:00"
content_hash: "sha256:727753fb6fa64abec1107ecca9a125d1bb7c6ad3fd15c47b1aa170f920df1a20"
---

# Pix Automático: o que é, como funciona e por que ERPs devem oferecer em 2026

## Por que o Banco Central criou o Pix Automático


O Pix Automático não surgiu do nada. É resultado de uma sequência deliberada de decisões regulatórias que o Banco Central vinha construindo desde o lançamento do Pix em novembro de 2020. O problema a resolver era claro: o débito automático tradicional funcionava bem dentro do mesmo banco, mas entre bancos diferentes dependia de um processo via CNAB com custo alto, prazo lento e cobertura limitada.


A linha do tempo: em dezembro de 2023, o BC publicou a primeira regulamentação do Pix Automático (Resolução BCB 360/2023). Em 16 de junho de 2025, foi lançado oficialmente ao público. Em 13 de outubro de 2025 (Resolução BCB 505/2025), tornou-se obrigatório em substituição ao débito automático interbancário para cobranças de PJ e entidades não reguladas. Em janeiro de 2026, o débito automático via boleto entre bancos diferentes foi encerrado.


## O que é Pix Automático


Pix Automático é uma modalidade de débito automático recorrente regulada pelo Banco Central. O cliente autoriza uma única vez, dentro do app do banco, que uma empresa específica debite valores da conta dele nas datas combinadas. Feito isso, as cobranças seguintes acontecem automaticamente, sem nova ação do pagador.


A autorização pode ser de valor fixo (R$ 49,90 por mês, sempre o mesmo) ou variável dentro de uma faixa (mínimo R$ 50, máximo R$ 200, por exemplo). As frequências cobrem semanal, mensal, trimestral, semestral e anual. Liquidação em D+0. Funciona de qualquer banco para qualquer banco.


Não confunda: Pix agendado é uma transação única programada. Pix Automático é débito autorizado de verdade — o cliente delegou uma vez e a operação roda sozinha daí pra frente.


## Pix Automático vs. boleto vs. cartão: comparativo direto


O custo é o mais visível. Uma empresa que processa 10 mil cobranças por mês paga, no boleto, algo entre R$ 15 mil e R$ 50 mil só em tarifas. No Pix Automático, o mesmo volume sai entre R$ 2 mil e R$ 5 mil. A velocidade muda o fluxo de caixa: cartão recebe em D+30; Pix Automático cai na conta no mesmo segundo.


Mas o ponto mais relevante é a cobertura. 60 milhões de brasileiros não têm cartão de crédito. A Hotmart reportou que 1 em cada 4 novos assinantes escolheu Pix Automático como método principal nos primeiros meses. Não é canibalização — é público novo entrando. O volume de Pix Automático cresceu 41% ao mês desde o lançamento.


## Pix Automático vs. débito automático tradicional: o que mudou de verdade


A diferença mais importante está na cobertura. O débito automático interbancário dependia de convênios bilaterais: uma empresa precisava firmar acordo separado com cada banco. O Pix Automático resolve isso estruturalmente — por ser construído sobre a infraestrutura do Pix, qualquer instituição participante (hoje mais de 900) aceita mandatos automaticamente. Uma única integração cobre todo o sistema financeiro brasileiro.


## Por que ERPs devem oferecer Pix Automático


ERPs são o hub financeiro da PME. Toda funcionalidade financeira que o ERP não cobre, a empresa busca em outro lugar — saída de receita e abertura pro concorrente. Quando o ERP integra Pix Automático como funcionalidade nativa, três coisas mudam:


-


Churn involuntário cai. Cartão tem 10% a 18% de falha por motivos alheios à intenção do cliente. Pix Automático é vinculado à conta corrente, que muda muito menos.


-


O dinheiro cai na hora. Em vez de D+30 do cartão ou D+2 do boleto, o cliente final recebe no momento da execução.


-


A operação para de depender de gente. Pix Automático executa sozinho e manda confirmação via webhook pro ERP dar baixa automática.


Não é teoria. A Cloud Gym, ERP para academias, implementou Pix Automático para mensalidades e eliminou tanto a inadimplência quanto as taxas de cartão que consumiam margem dos clientes todo mês. E tem um efeito estratégico: ERP com cobrança nativa retém o cliente dentro do ecossistema.


## Como implementar Pix Automático no ERP: visão geral


1. 1


Integração com uma plataforma ITP regulada. O ERP se conecta via API REST a uma plataforma como a Pluggy, que cuida da comunicação com os bancos.


2. 2


Criação do mandato de cobrança. Via API, o ERP define quem recebe, quanto (fixo ou faixa), frequência, início, fim e o que fazer se falhar.


3. 3


Autorização do cliente. O ERP gera um link e redireciona o cliente pra autorizar no app do próprio banco.


4. 4


Execução e baixa automática. Nas datas configuradas, a plataforma debita e manda webhook pro ERP atualizar o status da fatura.


Não é projeto de 6 meses. Um ERP com time de produto rodando coloca Pix Automático em produção em semanas.


## Erros comuns ao implementar Pix Automático no ERP


-


Não configurar o retry adequadamente, deixando a inadimplência subir desnecessariamente.


-


Não mapear o impacto na conciliação existente, criando faturas marcadas como pagas incorretamente e duplicidade.


-


Não comunicar a faixa de valor variável com clareza, gerando cancelamentos e reclamações no suporte.


-


Tratar o cancelamento de mandato como evento silencioso, deixando mandatos ativos enquanto as cobranças já pararam.


-


Subir pra produção sem testar os fluxos de exceção no sandbox.


-


Não definir o dono do mandato dentro do ERP pra monitorar o ciclo de vida (criado, ativo, pausado, cancelado, expirado).


## O que mudou em 2026 para cobrança recorrente via Pix


Janeiro de 2026 encerrou o débito automático via boleto entre bancos diferentes. Quem operava nesse modelo precisou migrar. As alternativas eram cartão (custo alto, cobertura limitada), boleto avulso (inadimplência alta) ou Pix Automático — o método que o próprio regulador indicou como substituto. ERPs que integrarem agora capturam a migração do cliente que perdeu o método antigo.


Comece pelo sandbox


A Pluggy tem a infraestrutura de Pix Automático disponível via API, com sandbox grátis pra testar antes de contratar. Fale com a gente e veja como colocar cobrança recorrente dentro do seu ERP.


[Ver changelog completo](https://www.pluggy.ai/#contato)
