---
schema_version: "1.0.0"
document_id: "3a3c28462b7bef61265a4e4191cd7810fa477b0d4c4fe173a4667ed5f44aa458"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-agosto-25-1"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:0cc5de73f9a5764fa1bf9204c397a13cb6d466067b7f988094302c00771ac175"
---

# [Atualizações de Produto] Agosto-25

[Back to All](https://docs.pluggy.ai/changelog)


> A cada mês, evoluímos nossos conectores e SDKs para que você tenha mais confiabilidade, velocidade e precisão nas integrações financeiras. 🚀
> Em agosto, avançamos em pontos estratégicos como redução de falhas em fluxos de criação, melhoria de performance em dados de pagamento e ajustes que trazem mais consistência aos conectores.
>
>
> No post de hoje, reunimos os principais destaques para você acompanhar de forma prática o que mudou e como essas melhorias impactam o seu dia a dia. Ao final, disponibilizamos um link com uma versão mais detalhada das mudanças.


###


**🔄 EfiPay**


Contas retornadas já na criação do item (via PIX API).
→ Precisa atualizar credenciais para novos escopos de token.
📘[Confira o passo a passo atualizado na documentação do EfiBank Empresas.](https://docs.pluggy.ai/docs/tutorial-efibank-empresas)


---


###


**⚙️ Open Finance – Sicoob**


Retry automático no endpoint resources (status 202/503).
→ Retenta até 5 min, reduz falhas na criação de item.
📘[Mais detalhes podem ser encontrados na documentação atualizada sobre o fluxo de sincronização.](https://docs.pluggy.ai/docs/item-lifecycle#synchronization-flow)


---


###


**🛠️ Open Finance – Conectores**


Campo products agora reflete apenas recursos suportados (Investments, Loans, Credit Cards, Accounts, Identity).
→ Evita confusão e tentativas de uso em features não suportadas.


---


###


**🕒 Open Finance – Histórico**


Retry automático em syncs históricos que retornam vazio.
→ Nova execução diária (até 1 ano), até dados completos serem recuperados.


---


###


**🆔 Open Finance – Identity**


Filtro em financialRelationships.accounts → só contas com consentimento.
→ Campo additionalInfo mapeado em endereço (ex.: ap/compl).
📘 Mais detalhes disponíveis na[documentação de referência](https://docs.pluggy.ai/reference/identity-retrieve) e no[guia de identidades](https://docs.pluggy.ai/docs/identities) .


---


###


**📉 Rico**


Removidos fluxos de ACCOUNTS e TRANSACTIONS.
→ Mantidos Identity e Investments (incl. Investment Transactions).
[📘 Mais detalhes disponíveis na documentação de cobertura de conectores.](https://docs.pluggy.ai/docs/connectors-coverage)


---


###


**💳 Bradesco PJ**


Melhorias em dados de pagamento:
Boletos/PIX enviados: recuperação 10x mais rápida + match mais preciso.
Correção do limite de 10K transações.


Quer ver uma versão mais detalhada do product updates, entendendo o que mudou e a importância das mudanças?[Acesse aqui.](https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-detalhada-agosto-25)
