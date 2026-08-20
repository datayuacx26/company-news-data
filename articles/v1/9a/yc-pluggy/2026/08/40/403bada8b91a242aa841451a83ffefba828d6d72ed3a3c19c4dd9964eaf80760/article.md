---
schema_version: "1.0.0"
document_id: "403bada8b91a242aa841451a83ffefba828d6d72ed3a3c19c4dd9964eaf80760"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-junho2026"
published_at: null
first_seen_at: "2026-08-14T04:18:01.345904+00:00"
fetched_at: "2026-08-14T04:18:02.940255+00:00"
content_hash: "sha256:726191dd3e0b57a3c74c72d3bb31ed368ff5683938578e9b7042d8a73c772896"
---

# Atualizações de produto | Junho/2026

[Back to All](https://docs.pluggy.ai/changelog)


> ⚠️
>
>
> ###
>
>
> **Breaking changes e depreciações**
>
>
> **Transição para cursor pagination no endpoint de transações.** Contas criadas a partir de junho de 2026 passam a usar por padrão o endpoint de transações com cursor pagination. O endpoint legado (v1) está em descontinuação gradual para essas contas.
>
>
> **O que fazer:** se a sua integração usa o endpoint legado, migre para o paginado por cursor. A documentação e os guias já refletem a mudança, e o SDK foi atualizado para suportá-lo.
>
>
> Contas criadas antes de junho de 2026 não são afetadas neste momento.


---


##


🚀 Novas Features


- → **Open Finance: novos campos de investimentos** — Novos campos disponíveis para investimentos via Open Finance, incluindo indicador de isenção fiscal (LCI/LCA/CRI/CRA), dados do devedor em CRI/CRA, periodicidade de remuneração, fator de preço para renda variável, classificação Anbima de fundos, número da nota de corretagem e informações de cupom para títulos do Tesouro Direto.
- → **Open Finance: novos campos de cartão de crédito** — Novos campos de categorização em cartões de crédito, incluindo tipo detalhado de tarifa, tipo de operação de crédito, informação de bandeira quando classificada como "outras" e motivo de limite zerado.
- → **Open Finance: novo código de contraparte em transações** — Transações de contas agora retornam o código ISPB da instituição de contraparte e uma descrição adicional para transações classificadas como "outros", melhorando a categorização.
- → **Open Finance: suporte a saldos reservados** — Novo campo` hasReservedBalance` e endpoint dedicado para consultar valores bloqueados ou reservados em conta (ex.: bloqueios judiciais, Pix agendado), oferecendo uma visão mais precisa do saldo disponível.
- → **Open Finance: campo**` gracePeriodDate` **para renda fixa** — Investimentos de renda fixa (CDB, LCI, LCA, CRI, CRA, Debêntures) agora retornam a data de carência via Open Finance.
- → **Códigos de erro mais específicos no endpoint de saldo** — O endpoint de saldo (balance) agora retorna códigos de erro mais granulares, em vez de agrupar a maioria das falhas sob um erro genérico, facilitando o tratamento pela sua integração.
- → **Limite do**` clientUserId` **padronizado** — O limite máximo do campo` clientUserId` foi padronizado em 500 caracteres em todos os endpoints da API (Item, Connect Token e Investments), eliminando inconsistências entre os limites anteriores.


---


##


💳 Pagamentos


- → **Pix Automático: respostas de pagamento mais completas** — As respostas de autorização e pagamento do Pix Automático agora incluem mais informações: dados da conta debitada, banco do recebedor, horários de autorização e atualização, motivo detalhado de erros e identificadores para conciliação (` consentId` ,` transactionIdentification` ).
- → **Pix Automático: correção na lógica de retentativas** — Corrigido o registro de datas em tentativas de retentativa com falha, que agora refletem a data de cada tentativa individual em vez da data original do pagamento. O sistema também passou a evitar novos agendamentos automáticos quando a instituição retorna erro de limite de período excedido.
- → **Pix Automático: correções no ambiente sandbox** — Corrigidos o erro 500 ao cancelar um agendamento e a falha ao gerar um payment request via dashboard, ambos no modo sandbox.
- → **Correção no status de pagamentos concluídos** — Corrigido um problema em que um pagamento já concluído podia ter seu status sobrescrito para erro quando uma tentativa (intent) subsequente expirava ou era rejeitada.
- → **Smart Transfers: correção em preautorizações** — Corrigido um erro que impedia a geração de preautorizações de Smart Transfer para clientes que selecionam determinadas instituições recebedoras.


---


##


🖥️ Dashboard


- → **Filtro por Customer ID em Payments** — A seção Payments > Customers agora permite filtrar registros diretamente pelo Customer ID, facilitando localizar pagamentos de um cliente específico.


---


##


🐛 Correções


- → **Cartões de crédito (Open Finance): sincronização parcial aprimorada** — A busca de faturas em sincronizações parciais agora acompanha corretamente o período das transações, aumentando significativamente a proporção de transações corretamente associadas à fatura.
- → **Conector Bradesco PJ: validação de usuário restaurada** — Corrigida a validação de usuários não suportados (USER_NOT_SUPPORTED), que havia deixado de funcionar corretamente.
- → **Conector Bradesco: mensagens de erro com acentuação corrigida** — Corrigida a codificação de caracteres especiais nas mensagens de erro (ex.: credenciais inválidas), que exibiam caracteres inválidos ao usuário final.
- → **Connect Widget: filtro por tipo de conector corrigido** — Corrigido um bug que fazia com que determinadas combinações de tipos de conector informadas ao widget não exibissem os conectores esperados.
- → **Connect Widget: modo de pessoa física/jurídica em pagamentos** — Corrigido um problema em que o widget não considerava corretamente o tipo de conta (pessoa física ou jurídica) do cliente ao iniciar um fluxo de pagamento, exigindo troca manual do modo.
- → **Connect Widget: sobreposição de notificação corrigida** — Corrigida uma sobreposição entre mensagens de notificação e a lista de contas disponíveis no widget, que podia dificultar a seleção da conta correta.
