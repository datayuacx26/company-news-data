---
schema_version: "1.0.0"
document_id: "83c894c09164680783cbe0fc87510e09313f6bc767e14a228c569eb4f3194f60"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-junho-25"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:dad8cbb54ed56925ba488b8e627fdd05b5f9b978942c3a806bf53058c768559b"
---

# [Atualizações de Produto] Junho-25

[Back to All](https://docs.pluggy.ai/changelog)


Fique por dentro das novidades da Pluggy no mês de Junho!


#


📡 Novos Conectores Open Finance


###


🌟 Incluímos 4 novos conectores regulados:


- Santander Corretora (812)
- Santander Corretora Empresas (813)
- PagueVeloz (Serasa) (814)
- PagueVeloz (Serasa) Empresas (815)


**Interessado? Fale com nossa equipe!**


---


#


🚀 Novas Features


##


🎲 Dados:


###


Dados de comprovante (boleto pago)


Agora, é possível visualizar dados de comprovante de boleto pago em mais uma instituição:


- Santander PJ


Ao todo, retornamos dados de comprovante de pagamento de boleto (multa, juros e descontos) nas seguintes instituições:


- Santander PJ
- Itaú PJ
- Bradesco PJ
- Inter PJ
- Sicredi PJ


Mais detalhes em nossa[documentação](https://docs.pluggy.ai/docs/transactions#boleto-metadata)


---


##


💳 Pagamentos


###


PIX Automático


Está disponível o produto de pix automático! Todas as informações para sua utilização estão em nossa documentação ([aqui](https://docs.pluggy.ai/docs/pix-automatico) e[aqui](https://docs.pluggy.ai/reference/payment-request-create-automatic-pix) ).


Converse com nosso time de vendas e venha testar esse produto!


---


#


✨ Melhorias


##


🎲 Dados


###


Descontinuação de conectores diretos (não-regulados)


- O conector **B3 CEI** está sendo descontinuado devido a medidas de segurança implementadas pela instituição.


- Este conector **não está disponível** no Open Finance regulado, dessa forma, não teremos mais acesso a conexões da plataforma B3 CEI.


###


XP Investimentos


- Realizamos diversas melhorias neste conector, visnaod maior estabilidade e redução de inconsistências nos dados coletados


###


SDK React Native


- Atualizamos nossa SDK de React Native para incluir parâmetros de Open Finance faltantes.


*Dúvidas?* Fale com nossa equipe!__


---


##


💳 Pagamentos


###


Diversas melhorias no produto Pix Automático:


- Inclusão de campo para que seja possível vincular um pagamento a um usuário (clientPaymentId)
- Possibilidade de cancelar um pagamento agendado específico ([documentação](https://docs.pluggy.ai/reference/cancel-automatic-pix-schedule) )
- Possibilidade de cancelar uma autorização de pagamento realizada com sucesso ([documentação](https://docs.pluggy.ai/reference/payment-request-cancel-automatic-pix-consent) )
- Melhorias em nosso app de pagamento para contemplar o fluxo de pix automático
- Inclusão de novos[webhooks](https://docs.pluggy.ai/docs/webhooks) para pix automático


---


#


📈 Bugs


###


🎲 Dados


- Portabilidade de Previdência:


- Corrigimos alguns bugs que estavam causando inconsistência na coleta de dados para o produto de portabilidade de previdência nos conectores XP e BTG.


- Dados de Empréstimo (OF):


- Corrigimos um defeito que estava causando um falso positivo nos dados de empréstimo.
