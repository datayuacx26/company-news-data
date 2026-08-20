---
schema_version: "1.0.0"
document_id: "641c1dbce744c34104760318a8d47c24d3d95aa0d52cf377a17d57df6c81e3db"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-julho2026"
published_at: null
first_seen_at: "2026-08-14T04:18:01.345904+00:00"
fetched_at: "2026-08-14T04:18:02.940255+00:00"
content_hash: "sha256:14a2665e8f5b7563c58e786cc76d73fb6d664599dc8256546566a2fd84526661"
---

# Atualizações de produto | Julho/2026

[Back to All](https://docs.pluggy.ai/changelog)


> 📘
>
>
> ###
>
>
> **Confira as principais novidades e melhorias da Pluggy em julho de 2026.**
>
>
> Destaques de julho
>
>
> - **Grande expansão da cobertura de pagamentos** : 6 conectores novos e 30 instituições que já existiam para dados passaram a suportar iniciação de pagamento.
> - Deprecação dos **conectores diretos para pessoa física** , com as conexões afetadas sendo removidas nos próximos meses.
> - Novos campos` billForecastDate` e` billClosingDate` em transações e faturas de cartão de crédito, e novo endpoint para remover regras de categorização.
> - Novo conector de dados via Open Finance: **Celcoin Baas** (PF, em BETA).
> - Diversas correções de estabilidade em cartões, investimentos, sincronizações e conectividade, além do SDK React Native com OAuth no iOS.


> ⚠️
>
>
> ###
>
>
> **Breaking changes e depreciações**
>
>
> **Conectores diretos para pessoa física entram em descontinuação.** Bradesco PF e Itaú já foram deslistados e o XP segue o mesmo caminho. O acesso fica restrito a um conjunto limitado de aplicações; para as demais, o conector deixa de ser retornado.
>
>
> **O que fazer:** migre essas conexões para o conector de Open Finance equivalente. **Não há migração automática** — o usuário final precisa refazer a conexão, e o histórico do item direto não é transferido. **As conexões afetadas serão removidas nos próximos meses** , então vale planejar a migração agora.
>
>
> **Conector Dock (810) descontinuado.** A marca saiu do diretório de participantes do Open Finance e o conector foi deslistado.
>
>
> ---
>
>
> **Relembrando — anunciado em junho: transição para cursor pagination no endpoint de transações.** Contas criadas a partir de junho de 2026 já usam por padrão o endpoint paginado por cursor, e o endpoint legado (v1) segue em descontinuação gradual para essas contas.
>
>
> **O que fazer:** se a sua integração ainda usa o endpoint legado, migre para o paginado por cursor. Contas criadas antes de junho de 2026 continuam sem ser afetadas por enquanto — avisaremos com antecedência antes de qualquer mudança para elas.


---


##


📡 Novos Conectores


- → **Grande expansão da cobertura de pagamentos** — 6 conectores novos, exclusivos de pagamento, com suporte a PIX imediato, agendado, Smart Transfer e Pix Automático: **Contabilizei** (PJ), **Nação BRB FLA** (PF), **BBC Digital** (PF), **Impact Bank** (PF), **Revolut** (PF) e **iFood Pago** (PJ).
- → **30 instituições passaram a suportar pagamento** — Instituições que já existiam para dados agora aceitam PIX imediato e agendado. Para PF e PJ: 6P Bank, Apex, Axiis Bank, Banco BRB, BPO Bank, Ella Bank, Health Cash, Hiperbanco, Imobi Bank, Lothus, Meirelles, Nitybank, Ovni, PagueVeloz Serasa, Rapidium Bank, Resolve Bank, Rhiza Bank, Simpbank, Snob Bank, Uze e Won Bank. Somente PF: Conta Universal, CRB Money, ESBank, Get Conta by Getnet, HausBank, iHold, POWPAY e Rico. Somente PJ: XP Empresas.
- → **Celcoin Baas (Open Finance, dados)** — Novo conector para pessoa física, em stage BETA, com suporte a Accounts, Transactions e Identity.


---


##


🚀 Novas Features


- → **Cartão de crédito: novos campos**` billForecastDate` **e**` billClosingDate` — As transações e faturas de cartão de crédito passam a retornar a data prevista da fatura e a data de fechamento, conforme a especificação do Open Finance Brasil. **Atenção:** estes campos são retornados apenas para conexões criadas após a implementação; conexões anteriores não retornarão esses valores.
- → **Novo endpoint para remover regras de categorização** —` DELETE /categories/rules/:id` permite excluir uma regra de categorização criada anteriormente, completando o ciclo de gerenciamento pela API.
- → **Open Finance: recuperação automática de conexões com falha** — Conexões de Open Finance que acumulam 5 erros consecutivos passam a ser reprocessadas automaticamente uma vez por dia, até que uma sincronização bem-sucedida as recupere. Antes, essas conexões ficavam paradas e exigiam reconexão manual.


---


##


💳 Pagamentos


- → **Pix Automático: cobrança com consentimento cancelado corrigida** — Corrigido um caso em que uma cobrança de Pix Automático podia ser processada mesmo após o consentimento ter sido cancelado.


---


##


🔔 Webhooks


- → **Correção em retentativas em massa** — Corrigido um cenário em que um volume grande de retentativas deixava de ser enviado.


---


##


🖥️ Dashboard


- → **Logo para o modo escuro** — Agora é possível enviar um logo específico para o modo escuro do Connect, além do logo principal. Se nenhum for enviado, o logo do modo claro é usado nos dois temas.
- → **Apps de desenvolvimento e produção separados na lista de webhooks** — Corrigido um problema em que aplicações dos dois ambientes apareciam misturadas ao configurar webhooks.
- → **Correções em Pagamentos** — Corrigidos a data exibida em Pix agendado, a criação de Pix Automático pelo dashboard e a listagem de recebedores.


---


##


🐛 Correções


- → **Banco do Brasil: saldo do Rende Fácil** — Corrigido um caso em que o saldo aparecia zerado quando o valor estava aplicado no Rende Fácil. O BB retorna` availableAmount = 0` nessa situação, e agora o valor aplicado é considerado no saldo da conta, como já acontecia no Itaú.
- → **Cartões de crédito: datas de fatura e isolamento de erros** — Corrigidas as datas de fechamento e vencimento quando a instituição retorna faturas fora de ordem cronológica, e corrigido um caso em que um cartão com erro esvaziava as transações dos demais cartões do mesmo item.
- → **Investimentos** — Corrigida a falha em contas BBA do Itaú PJ e melhorado o tratamento de erros de rate limit no conector de investimentos.
- → **Sincronizações mais resilientes** — Evitadas execuções infinitas em conexões com` CONNECTION_ERROR` ; corrigidas as datas usadas em sincronizações parciais após um erro de mesclagem; adicionada retentativa inteligente para primeiras conexões do Sicoob que retornavam sem dados; corrigidas transações ausentes no Sicoob (OF), no sync diário do BB PJ quando` ACCOUNTS` não estava entre os produtos, e na Caixa PJ quando a data inicial era anterior à abertura da conta.
- → **Itaú: lançamentos SISPAG desmembrados** — Corrigido o tratamento de lançamentos de salário do SISPAG que passavam a ser retornados de forma desmembrada após a habilitação do AUT MAIS.
- → **Connect Widget: botão de conectar** — Corrigidos casos em que o texto do botão de conectar não aparecia ou o botão não respondia.
- → **SDK React Native: conexões OAuth no iOS** —`[\[email protected\]](https://docs.pluggy.ai/cdn-cgi/l/email-protection)` adiciona a prop` forceOauthInBrowser` . Com` false` , o OAuth abre em uma WebView dentro do app em vez do navegador do sistema, resolvendo conexões de conectores regulados que ficavam presas em` WAITING_USER_INPUT` até expirar no iOS. O padrão continua` true` , então a mudança é opt-in.
