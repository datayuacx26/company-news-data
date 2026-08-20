---
schema_version: "1.0.0"
document_id: "44e32c04ef8e5d4689f7c4deb08543eda7f6e44c6d9d99e1e90f7147ab0fb838"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/atualizacoes-produto-fevereiro-2026"
published_at: "2026-02-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:9b5697f3c3e0ef42cd7e0c292ce824883524896aed011637ccf4e3b601c4046b"
---

# Atualizações de Produto — Fevereiro 2026: novo dashboard, conectores e pagamentos recorrentes

Fevereiro foi o mês do novo Dashboard Pluggy. Depois de meses de trabalho, lançamos uma experiência completamente redesenhada — com foco em reduzir o tempo de integração para novos clientes e dar mais visibilidade operacional para quem já está em produção.


## Novo Dashboard: do zero à produção em menos tempo


O novo dashboard foi construído com três objetivos principais: facilitar o onboarding, dar visibilidade da operação em tempo real, e permitir gestão sem precisar da API para cada ação.


-


Ambientes separados — alterne entre Development e Production no menu lateral


-


Onboarding guiado em 4 etapas — connect token, widget, webhooks


-


Tour interativo pelas principais áreas


-


Visão geral com conexões diárias, execuções e distribuição por instituição


-


Due Diligence digital com upload de documentos


-


Personalização do widget com preview em tempo real


-


Gestão de equipe com convites e papéis


-


Retentativa de webhooks em lote por intervalo de tempo


-


Alertas automáticos de conectores offline ou instáveis


## Novos conectores Open Finance


Três novas instituições foram adicionadas à cobertura regulada da Pluggy:


-


Itaú PF — conector regulado via Open Finance para pessoa física


-


Bradesco PF — conector regulado via Open Finance para pessoa física


-


Inter PJ para MEI — microempreendedores individuais atendidos pelo conector regulado do Inter PJ


## Pagamentos: recorrência com mais controle


As funcionalidades de Pix Automático ficaram mais robustas com três novidades:


-


Agendamento de pagamentos para datas futuras


-


Retentativa automática em caso de falha — sem intervenção manual


-


Identificação do primeiro pagamento de uma recorrência


## Webhooks com mais controle


-


Headers de autenticação personalizados


-


Retentativa por intervalo de tempo


-


Desativação automática de URLs inativas


-


Limite configurável de webhooks por tipo de evento


## Melhorias na API de dados


O campo accountSubtype agora é retornado na resposta da API. Filtro por data de criação de transações, prevenção de conexões duplicadas com MFA, e validação de CNPJ antes de conectar no Bradesco PJ.


## Correções


Datas inválidas em transações de cartão (Caixa) e boletos (Bradesco PJ), extrato PDF restaurado no Itaú PJ, transações duplicadas eliminadas no Banrisul e Banco do Brasil, e correções de dados em 10 instituições.


Changelog completo


Para a lista completa de todas as mudanças de fevereiro, acesse o changelog de fevereiro.


[Ver changelog completo](https://www.pluggy.ai/changelog/fevereiro-2026)
