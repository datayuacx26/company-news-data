---
schema_version: "1.0.0"
document_id: "7721ac9a7c225703902ea8745838b661aa24a5c59ad24d50126aeb993269d794"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-fevereiro-25"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:62f11c87687bb0a1c2b312c74696fef2f291e05cd90ec59aea424072d1725947"
---

# [Atualizações de Produto] Fevereiro-25

[Back to All](https://docs.pluggy.ai/changelog)


#


🚀 Features


###


Novos webhooks para gestão das transações:


- ` transactions/created` : serão enviadas notificações sempre que novas transações forem coletadas na conta;
- ` transactions/updated` : serão enviadas notificações sempre que houver mudanças nas transações existentes (descrição, valor etc). Serão informados os IDs de cada transação alterada;
- ` transactions/deleted` : serão enviadas notificações sempre a instituição remover alguma transação. Serão informados os IDs de cada transação deletada.


Para maiores detalhes, acesse nossa documentação: 🔗[Guia de Sincronização de Transações](https://docs.pluggy.ai/docs/transactions#how-to-synchronize-and-merge-transactions) .


---


###


Novidade sobre dados de pagamento de boletos!


- Agora, os conectores Itaú PJ e Bradesco PJ retornam dados referentes a comprovante de pagamento de boletos.
- Isso significa que, em transações relacionadas a boletos, retornaremos informações no objeto` boletoMetadata` como:


- Código de barras;
- Linha digitável do boleto;
- Status do boleto (pago, pendente, etc.);
- Dados de multas, juros, descontos etc.


Para maiores detalhes, acesse nossa documentação: 🔗[dados de pagamento de boletos](https://docs.pluggy.ai/docs/transactions#transaction-payment-data-schema)


---


###


Emissão de boletos (Inter PJ)!


- Temos uma nova API para emissão de boletos!
- Atualmente, está apenas disponível para Inter PJ.


Para maiores detalhes, acesse nossa documentação: 🔗[API de emissão de boletos](https://docs.pluggy.ai/docs/boleto-management-api)


---


#


✨ Melhorias


- Agora, recuperamos as **transações** referentes a *Aplicação e Resgate Automáticos* no **Itaú PJ** ;
- Aumentamos o tempo para realização do consentimento nas conexões Open Finance para 20 minutos;
- Adicionamos a mensagem de erro recebida do provedor nos eventos de webhooks do tipo *erro (item/error)* ;
- Implementamos um novo endpoint para reembolsos automáticos nos casos em que o dinheiro fica na smart account.


---


#


📈 Bugs


- Correções diversas na **API de Pagamentos** , visando maior estabilidade e visibilidade em cenários de erro;
- Resolvidos problemas com **Bradesco PF e XP Investimentos (não regulado)** que impactavam as conexões e atualizações de contas;
