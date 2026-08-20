---
schema_version: "1.0.0"
document_id: "ee8ab167f1599c2a3c5937c0744ebbd9d75869395b17623d891c4eae667b792c"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-mar%C3%A7o2026"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:4b3ae66edf0989ced8e71660219a5b3f6c06af7929a4f9d0b2d6d622dea964f0"
---

# Atualizações de Produto | Março/2026

[Back to All](https://docs.pluggy.ai/changelog)


> 📘
>
>
> ##
>
>
> Confira as principais novidades e melhorias da Pluggy em março de 2026.
>
>
> **Destaques de março**
>
>
> - Expansão significativa das funcionalidades de pagamentos, com nova seção completa no dashboard e evolução da API.
> - Avanços em Pix Automático e Smart Transfers, com melhorias de gestão, eventos e consistência dos fluxos.
> - Atualizações e maior estabilidade em conectores, incluindo BB Prev e CaixaPrev.
> - Evolução do dashboard com novo onboarding, analytics mais completos e melhorias na visualização de dados.
> - Diversas correções e melhorias de estabilidade em sincronização, webhooks e qualidade dos dados.
>
>
> ⚠️ Algumas funcionalidades estão sendo liberadas aos poucos e podem variar dependendo da instituição, tipo de integração e ambiente (sandbox ou produção). Se algo não aparecer por aí ainda, é só chamar a gente.


##


💳 Pagamentos


Março trouxe uma expansão significativa das funcionalidades de pagamento, com uma seção completa no dashboard e novos recursos na API.
→ **Seção de Pagamentos no Dashboard** — Agora você pode gerenciar suas solicitações de pagamento diretamente pelo dashboard, com criação, busca, filtros, paginação e visualização detalhada de cada solicitação.
→ **Gestão de Recebedores **— Cadastre, edite e busque recebedores de pagamento por nome ou CPF/CNPJ, tudo pelo dashboard. →** Gestão de Pagadores** — Cadastre e gerencie seus pagadores (customers) com uma interface dedicada no dashboard.
→ **Gestão de PIX Automático no Dashboard** — Gerencie seus agendamentos de PIX Automático diretamente pelo dashboard: visualize ocorrências, acompanhe o status de cada pagamento e cancele consentimentos ou agendamentos individuais.
→ **Smart Transfers** — Nova seção de Smart Transfers no dashboard para gerenciar transferências inteligentes.
→ **Endpoint /balance** — Novo endpoint para consultar o saldo disponível no iniciador de pagamentos.
→ **Metadados de boleto (Inter PJ)** — Transações de boleto do Inter PJ agora incluem metadados detalhados do pagamento.
→ **Webhook de expiração** — Você agora recebe um webhook quando uma solicitação de pagamento expira automaticamente.
→ **Evento CONSUMED no PIX Automático** — O evento CONSUMED agora é tratado corretamente no fluxo de PIX Automático.
→ **Correção no campo supportsAutomaticPix** — O campo supportsAutomaticPix agora retorna os valores corretos para cada instituição.
→ **Correção de caractere inválido** — Corrigido um problema com caracteres inválidos no nome do pagador em transações PIX Automático.


---


##


📡 Novos Conectores


→ **BB Prev (nova versão)** — O conector BB Prev foi migrado para uma versão mais moderna e estável.
→ **CaixaPrev (atualização)** — O conector CaixaPrev foi revisado e atualizado para maior confiabilidade.


---


##


🚀 Novas Features


→ **Transações pendentes (Open Finance)** — Transações com status "pendente" agora são automaticamente atualizadas para "postada" quando confirmadas pela instituição.
→ **BrasilPrev** — suporte a multifundos — Agora você pode visualizar investimentos separados por fundo no BrasilPrev.
→ **Ordenação alfabética no Widget** — As instituições no Pluggy Connect Widget agora são exibidas em ordem alfabética para facilitar a busca.
→ **Pagamentos no modelo de fatura (Open Finance)** — A lista de pagamentos agora é retornada no modelo de fatura via Open Finance.
→ **Banner de upgrade para v2** — Usuários do dashboard v1 agora veem um banner com convite para migrar para a nova versão.


---


##


🖥️ Dashboard


→ **Novo onboarding** — Experiência de boas-vindas redesenhada com etapas guiadas, convite de colaboradores e animações de entrada.
→ **Due Diligence em tela cheia** — O formulário de Due Diligence agora é um wizard multi-etapas em tela cheia, mais intuitivo e organizado.
→ **Checklist de Produção** — O checklist de prontidão para produção foi redesenhado como um card "Solicitar Acesso à Produção" com verificação automática de saúde da integração.
→ **Overview para Sandbox** — A página de visão geral foi redesenhada para usuários sandbox, com dicas de integração e página de analytics.
→ **Estatísticas do Syncer** — Novas métricas de sincronização com gráfico de retentativas, KPI de itens atualizáveis e deduplicação por item.
→ **Filtro por conector** — Agora você pode filtrar as estatísticas de sincronização por conector específico.
→ **Gráfico de itens por instituição** — Novo gráfico com visualização em lista ou gráfico e filtro multi-select de conectores.
→ **Lag por conector** — Gráfico de donut mostrando o atraso de atualização por conector, com cores da marca de cada instituição.
→ **Payload de eventos** — Agora você pode visualizar o payload completo de eventos de webhook diretamente no drawer de detalhes.
→ **Convites pendentes** — Nova interface para gerenciar convites de equipe pendentes.


---


##


🔔 Webhooks


→ **Correção de webhooks duplicados** — Webhooks do tipo transactions/created não são mais enviados quando a execução não cria uma transação realmente nova.


---


##


🐛 Correções


→ **Correções de dados em múltiplas instituições** — Corrigidos problemas de transações e contas não retornadas ou duplicadas em: Caixa Econômica Federal, Sicredi, Nubank, Santander, Inter, Banco do Brasil, C6, Banrisul, Mercado Pago, Bradesco, Wise, 99 Pay, Itaú e Sicoob.
→ **Retentativa automática para erros de merge** — Implementado mecanismo de retentativa para erros consistentes de merge, reduzindo falhas de sincronização.
→ **Itaú PJ — credenciais inválidas** — Corrigido erro que retornava uma mensagem genérica ao invés de indicar credenciais inválidas no Itaú PJ.
→ **Exclusão de itens** — A ação de excluir item agora remove corretamente o item via API.
→ **Investimentos Open Finance** — Corrigido problema onde investimentos eram misturados quando novos apareciam em ordem diferente.
→ **Transações Banrisul** — Corrigida a ordenação de transações que eram retornadas em ordem crescente pela instituição.
→ **Remoção do Banco Paulista** — O Banco Paulista foi removido da lista de conectores Open Finance por descontinuação.


---


##


📚 Documentação


→ **Playground atualizado** — O Playground foi atualizado para facilitar o teste de cada tipo de pagamento disponível.
