---
schema_version: "1.0.0"
document_id: "a15b8c1380aade6922636dffcd8c1c786cfe63eecb5339ac4efd419d23464446"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-abril-25"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:99d72aabf07515f1d8bce1e163076ed0fa0aaf72d8b977b4218bb157b45528fc"
---

# [Atualizações de Produto] Abril-25

[Back to All](https://docs.pluggy.ai/changelog)


Fique por dentro das novidades da Pluggy no mês de Abril!


#


🚀 Features


###


Dados de comprovante (boleto pago)


Agora, é possível visualizar dados de comprovante de boleto pago em mais uma instituição:


- Inter PJ


Ao todo, retornamos dados de comprovante de pagamento de boleto (multa, juros e descontos) nas seguintes instituições:


- Itaú PJ
- Bradesco PJ
- Inter PJ


Mais detalhes em nossa[documentação](https://docs.pluggy.ai/docs/transactions#boleto-metadata)


---


#


✨ Melhorias


###


🎲 Dados


- Atualizamos em nossa Widget as instruções para conexão com Inter PF:


- Itaú PJ: melhoria na performance do conector, o que representa uma redução de 15% no tempo de conexão.
- Clear Investimentos: Devido a diversas instabilidades que inviabilizaram o uso, tivemos que encerrar o suporte ao conector Clear não regulado.


- O conector continua disponível no acesso **regulado** ;
- Mais informações sobre os conectores disponíveis, acesse nossa documentação:


- [conectores "regulado"](https://docs.pluggy.ai/docs/open-finance-regulated#institutions-supported-by-open-finance)
- [conetores "não regulado"](https://docs.pluggy.ai/docs/connectors-coverage)


###


💳 Pagamentos


- Adicionamos novas instituições para fluxos de pagamento:


- Dock
- 99pay
- Swap
- Azimut
- BMS Sociedade de Crédito Direto S/A
- ModalMais trader
- Listo
- Iugu


---


#


📈 Bugs


###


🎲 Dados


- Itaú PJ: foram resolvidos problemas referentes a:


- transações sem *payment data* e;
- conexões que não estavam sendo atualizadas diariamente.


- Bradesco PJ: corrigimos alguns erros no fluxo de recaptcha que estavam impactando as atualizações automáticas neste conector.


###


💳 Pagamentos


- Resolvemos um bug o qual inviabilizava o cancelamento de agendamentos futuros.
