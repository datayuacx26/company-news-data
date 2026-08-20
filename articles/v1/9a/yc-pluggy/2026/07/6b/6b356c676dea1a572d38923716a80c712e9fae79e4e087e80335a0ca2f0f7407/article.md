---
schema_version: "1.0.0"
document_id: "6b356c676dea1a572d38923716a80c712e9fae79e4e087e80335a0ca2f0f7407"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-setembro"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:03cfaa00cd26747d0fe0aa29a81bffc7057ce7ac49cec4ac34f8291f96df69a8"
---

# Atualizações de Produto | Setembro

[Back to All](https://docs.pluggy.ai/changelog)


> Outubro foi um mês cheio de avanços no ecossistema Pluggy! 🌟
>
>
> Tivemos melhorias importantes em conectores Open Finance, otimizações de performance e novos recursos que tornam as integrações mais estáveis, precisas e fáceis de usar.
> Entre os destaques estão ajustes em mensagens de aviso de cartões de crédito, correções de lógica em transações e saldos, suporte a deep links no OAuth e novos conectores disponíveis em BETA.
> Abaixo, você confere o resumo das principais atualizações que chegaram para deixar sua integração com a Pluggy ainda mais confiável. 👇


###


\[OF\] Credit Card Warning


→ Avisos de cartão sem consentimento/indisponível sempre visíveis.
→ Mensagem separada por tipo: no consent ou unavailable.


###


\[Santander OF\] Pagination Fix


→ Paginação ajustada: contagem correta mesmo com page-size=100.
→ Limite adaptativo (500 páginas / 200 k transações).


###


\[OF\] Avoid Duplicates


→ Adicionados Nubank PF/PJ à whitelist de bloqueio de duplicidade (CPF/CNPJ).
→ Lista de instituições revisada e documentada.


###


\[Sandbox\] Múltipla Alçada


→ Fluxo funcional no widget; permite teste com múltiplos CPFs.
→ statusDetail indica quando a conta precisa de autorização.


###


\[OF\] Credit Card Future Transactions


→ Corrigido status incorreto de transações futuras.
→ Remove billId se billPostDate > data atual → marca como PENDING.


###


\[OF\] Closing Balance


→ closingBalance = availableBalance + blockedBalance.
→ Agora reflete corretamente o saldo contábil.


###


\[API\] Deep Links OAuth


→ oauthRedirectUri agora aceita deep links.
→ Facilita autenticação e retorno direto em apps mobile.


###


\[OF\] New Connectors (BETA)


→ Dock PF – Accounts, Transactions, Identity
→ QI SCD PF – Accounts, Transactions, Loans, Identity
→ QI SCD PJ – Accounts, Transactions, Loans, Identity


[Veja a versão detalhada aqui.](https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-detalhada-setembro#/)
