---
schema_version: "1.0.0"
document_id: "59150e0f1542e5bd1198253e7f6f20cdae2ea0cc399a9bab0d9990f347be519d"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-mar%C3%A7o-25"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:be71dffe56d9f3562b355c5619964212e2b61fa5045249e5c46deaa5b01a4b4a"
---

# [Atualizações de Produto] Março-25

[Back to All](https://docs.pluggy.ai/changelog)


Fique por dentro das novidades da Pluggy no mês de Março!


#


📡 **Novos Conectores Open Finance**


###


🌟 EQI Investimentos


**Interessado? Fale com nossa equipe!**


---


#


✨ Melhorias


###


🎲 Dados


- Ajustes visando a estabilização do conector Sicoob PJ


###


💳 Pagamentos


- Adicionamos informações de *juros* e *multa* em nossa api de emissão de boletos (feature disponível apenas para Inter PJ. Mais detalhes em nossa[documentação](https://docs.pluggy.ai/docs/boleto-management-api) .
- Maior clareza dos erros retornados em nossa API de pagamentos.
- Melhorias diversas em nossa[documentação](https://docs.pluggy.ai/docs/payments-overview-1) de pagamentos.
- Por questões regulatórias, agora os dados de conta do recebedor não serão mais exibidos por completo. Essa alteração ocorre no objeto accounts do endpoint` /recipient` , o qual terá os dados exibidos com máscara.


- Dados a serem mascarados:


- agência,
- número da conta,
- tipo da conta,
- cpf completo,
- data de abertura da conta.


- Exemplo de como será retornada essa informação mascarada:


- ` "account": { "type": "****" "number": "******112", "branch": "**1" }`


---


#


📈 Bugs


###


🎲 Dados


- Resolvemos casos de transações deletadas em Itaú PF, Itaú PJ, Bradesco PJ.
- Resolvemos diversos casos de investimentos não retornados na XP.
- Consertamos um erro do Open Finance onde algumas transações de cartão de crédito estavam consolidadas, porém sem vínculo com nenhuma fatura.
