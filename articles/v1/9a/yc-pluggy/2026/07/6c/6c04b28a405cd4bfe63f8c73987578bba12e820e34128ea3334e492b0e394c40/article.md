---
schema_version: "1.0.0"
document_id: "6c04b28a405cd4bfe63f8c73987578bba12e820e34128ea3334e492b0e394c40"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-maio2026"
published_at: null
first_seen_at: "2026-07-22T21:31:04.675210+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:a0f8af2a4fa7dd306d9be02e98f8f56f47fa654e0fe4f8d5c6b56e6f3a38a6fd"
---

# Atualizações de produto | Maio/2026

[Back to All](https://docs.pluggy.ai/changelog)


> 📘
>
>
> ##
>
>
> Confira as principais novidades e melhorias da Pluggy em maio de 2026.
>
>
> Destaques de maio
>
>
> - Suporte ao novo CNPJ alfanumérico, garantindo compatibilidade com o novo padrão da Receita Federal sem necessidade de ajustes nas integrações.
> - Expansão do Open Finance, com novos tipos de operações de crédito, dados de identidade mais completos e suporte a conexões com grande volume de contas.
> - Evolução da API e do Dashboard, com novos filtros de transações, paginação por cursor, filtros avançados de webhooks e melhorias no upload de documentos.
> - Melhorias no Connect Widget, incluindo suporte ao fluxo de Múltipla Alçada e orientações mais claras para cenários de limite de recursos.
> - Diversas correções de estabilidade em conectores, transações, pagamentos, webhooks e sincronização, além de atualizações nos SDKs e na documentação.


##


🚀 Novas Features


- → **Suporte ao novo CNPJ alfanumérico** — A API Pluggy agora aceita e processa CNPJs no novo formato alfanumérico exigido pela Receita Federal, sem necessidade de ajustes no seu sistema de integração.
- → **Open Finance: novos tipos de operação de crédito disponíveis** — Agora você pode recuperar dados de financiamentos, financiamento de faturas e crédito em conta corrente via Open Finance, ampliando significativamente a cobertura de dados de crédito disponíveis para análise.
- → **Open Finance: campos de identidade ampliados** — Os dados de identificação, qualificação e relações financeiras do Open Finance agora retornam campos adicionais que anteriormente estavam ausentes, aumentando a completude das informações de identidade do usuário.
- → **API: novos parâmetros de filtro em transações** — Novos campos de filtro e opções de paginação foram adicionados à API de transações, permitindo consultas mais precisas e flexíveis ao histórico de dados.
- → **Open Finance: suporte a itens com alto volume de contas** — Conexões Open Finance com grande número de contas e recursos vinculados agora têm.
- dados acessíveis em todos os endpoints, eliminando lacunas de cobertura para clientes com carteiras amplas.
- → **Widget: tela de Múltipla Alçada** — O Connect Widget agora inclui uma tela dedicada ao fluxo de Múltipla Alçada, permitindo que usuários em processos que exigem aprovação de múltiplos responsáveis concluam a conexão sem atrito.
- → **Widget: orientação para o status**` RESOURCES_LIMIT_EXCEEDED` — O Connect Widget agora exibe uma mensagem clara e orientação ao usuário quando uma conexão Open Finance atinge o limite de recursos, melhorando a experiência em cenários de alto volume.
- → **Sandbox: maior volume de transações para testes** — O ambiente sandbox agora retorna um volume maior de transações históricas, facilitando o desenvolvimento e testes de funcionalidades que dependem de histórico transacional extenso.


---


##


📡 Novos Conectores


- → **Banrisul PJ disponível para todos os clientes** — O conector Banrisul para pessoa jurídica agora está disponível para todos os clientes da Pluggy, sem necessidade de habilitação manual pela equipe de suporte.


---


##


💳 Pagamentos


- → **PixAuto: erro explícito ao cancelar pagamento na mesma data** — Ao tentar cancelar um pagamento PixAuto agendado para o dia atual, a API agora retorna um erro claro e descritivo, evitando comportamento inesperado ou falha silenciosa.


---


##


🔔 Webhooks


- → **Correção no timing do evento**` item/created` **para Open Finance** — Corrigido um problema em que o evento` item/created` era disparado antes que a conexão Open Finance fosse aprovada pela instituição financeira, causando notificações prematuras no seu sistema.


---


##


🖥️ Dashboard


- → **Filtragem por data e hora nos webhooks** — O painel de webhooks agora permite filtrar eventos por data e hora com precisão de minutos, simplificando a investigação de eventos em janelas de tempo específicas.
- → **Limite de upload de documentos aumentado para 20 MB** — O formulário de Due Diligence agora aceita arquivos de até 20 MB, eliminando erros ao enviar documentos maiores como extratos e contratos.
- → **Paginação por cursor nas transações** — A listagem de transações no Dashboard passou a usar cursor pagination, tornando a navegação mais rápida e estável em contas com grande volume de dados.


---


##


🐛 Correções


- → **Reprocessamento histórico de dados Open Finance restaurado** — A funcionalidade de retry automático de dados históricos em conexões Open Finance foi reativada, garantindo maior completude nos dados retornados para itens recém-conectados.
- → **EnrichmentAPI: erros de autenticação corrigidos** — Corrigidos erros que ocorriam ao consultar a Enrichment API com determinadas combinações de credenciais.
- → **Widget de Pagamentos: lista de instituições PJ corrigida** — Corrigido um problema em que, ao iniciar um pagamento como pessoa jurídica, a lista de instituições exibida era de instituições PF.
- → **Banrisul: seleção de empresa e histórico de transações** — Corrigida a seleção incorreta de empresa no conector Banrisul e o retorno de transações no modo de sincronização histórica.
- → **Bradesco PF e Bradesco PJ: conectividade restaurada** — Resolvidas instabilidades que impediam usuários de conectar e atualizar contas Bradesco PF e Bradesco PJ.
- → **Itaú PJ: erros de conexão resolvidos** — Corrigidos erros intermitentes que impediam a atualização automática de contas Itaú PJ.
- → **XP: código SUSEP ausente em produtos de investimento** — Corrigida a ausência do código SUSEP no retorno de produtos de investimento do conector XP, afetando a classificação regulatória de ativos.
- → **Widget: botões de ação não exibidos corretamente** — Corrigido um problema de renderização em que os botões de ação desapareciam em determinadas telas do Connect Widget.
- → **Dados de transações corrigidos para múltiplas instituições** — Corrigidos problemas de retorno de transações de conta corrente, cartão de crédito e investimentos para as seguintes instituições: Bradesco, Santander, Itaú, Banco do Brasil, Nubank, BTG Pactual, Sicredi, Sicoob, Caixa Econômica Federal, Caixa Tem, Banrisul, XP Banking, InfinityPay, Inter e C6.


---


##


📚 Documentação


- → **SDK Node.js: cursor pagination completo** — O SDK Node.js foi atualizado com os campos de cursor que estavam ausentes, habilitando uso completo da cursor pagination em listagens de transações.
- → **SDK Java: suporte a cursor pagination** — O SDK Java foi atualizado para suportar cursor pagination, alinhando-o às capacidades atuais da API.
- → **Quickstart e dependências atualizados** — O guia de quickstart foi revisado com melhorias de conteúdo e as dependências do pluggy-sdk foram atualizadas para as versões mais recentes.
