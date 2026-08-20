---
schema_version: "1.0.0"
document_id: "a9bd9c8ec773fba3bbd4883894a137850d04cac5e45dea27e410d4316558d886"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/atualizacoes-produto-marco-2026"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:efc73f4e09c7fdd3ad03b8dbac819e82f310cb47d378eb40810f0a711549e905"
---

# Atualizações de Produto — Março 2026: pagamentos ganham protagonismo no dashboard

Março de 2026 foi marcado pela chegada de uma área completa de pagamentos no dashboard da Pluggy. O objetivo é simples: tudo que antes exigia chamadas diretas à API agora está acessível em uma interface dedicada — sem abrir o terminal, sem precisar de um dev para cada ação operacional.


## Pagamentos: da API para a interface


A nova seção de Pagamentos no dashboard centraliza o gerenciamento de solicitações, recebedores, pagadores e consentimentos de PIX Automático. Você consegue criar, buscar, filtrar e visualizar cada solicitação com paginação completa.


-


Gestão de Recebedores — cadastre, edite e busque por nome ou CPF/CNPJ


-


Gestão de Pagadores — interface dedicada para customers


-


PIX Automático — visualize ocorrências, acompanhe status, cancele consentimentos


-


Smart Transfers — nova seção para gerenciar transferências inteligentes


-


Endpoint /balance — consulte o saldo disponível no iniciador de pagamentos


## Novos conectores


Dois conectores de previdência foram atualizados: BB Prev migrado para uma versão mais moderna e estável, e CaixaPrev revisado para maior confiabilidade. Ambos já estão disponíveis para todas as integrações.


## Open Finance: transações pendentes viram postadas


Uma melhoria relevante para quem usa dados em tempo real: transações com status pendente agora são automaticamente atualizadas para postada quando a instituição confirma. Menos tratamento manual no código da aplicação.


Outras novidades: suporte a multifundos no BrasilPrev, ordenação alfabética no Pluggy Connect Widget, e pagamentos no modelo de fatura via Open Finance.


## Dashboard mais completo


Além da área de pagamentos, o dashboard recebeu melhorias de experiência:


-


Novo onboarding com etapas guiadas e animações


-


Due Diligence em tela cheia — wizard multi-etapas mais intuitivo


-


Checklist de Produção redesenhado com verificação automática de saúde da integração


-


Estatísticas do Syncer com gráfico de retentativas e KPIs de sincronização


-


Payload de eventos visível diretamente no drawer de detalhes


-


Filtro de estatísticas por conector específico


## Correções


Corrigidos problemas de transações e contas em 14 instituições: Caixa, Sicredi, Nubank, Santander, Inter, Banco do Brasil, C6, Banrisul, Mercado Pago, Bradesco, Wise, 99 Pay, Itaú e Sicoob. Também corrigida a duplicação de webhooks transactions/created e a remoção do Banco Paulista da lista de conectores.


Changelog completo


Para a lista completa de todas as mudanças de março, acesse o changelog de março.


[Ver changelog completo](https://www.pluggy.ai/changelog/marco-2026)
