---
schema_version: "1.0.0"
document_id: "3e4feca2a1ab2ec655d2a3c1e0b78f622df208cfeb93b06f46a997cfae719a61"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-maio-25"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:b7ba6f7075b9923aadf8586830dd4c12d0cd95ee59d72562efbab80a8d2b98a1"
---

# [Atualizações de Produto] Maio-25

[Back to All](https://docs.pluggy.ai/changelog)


Fique por dentro das novidades da Pluggy no mês de Maio!


#


📡 Novos Conectores Open Finance


###


🌟 Rede Celcoin (PF & PJ)


**Interessado? Fale com nossa equipe!**


---


#


🚀 Novas Features


##


🎲 Dados:


###


Dados de comprovante (boleto pago)


Agora, é possível visualizar dados de comprovante de boleto pago em mais uma instituição:


- Sicredi PJ


Ao todo, retornamos dados de comprovante de pagamento de boleto (multa, juros e descontos) nas seguintes instituições:


- Itaú PJ
- Bradesco PJ
- Inter PJ
- Sicredi PJ


Mais detalhes em nossa[documentação](https://docs.pluggy.ai/docs/transactions#boleto-metadata)


---


###


Gestão de webhooks via Dashboard Pluggy


Implementamos no Dashboard uma modificação que permite ao usuário ver **detalhes avançados** de um evento de webhook (como **status** e o **detalhe do erro** , em caso de falha), além da opção de **reenvio** .
Abaixo, uma breve demonstração da nova feature:


---


##


💳 Pagamentos


###


Novo campo: identificador do pagamento na instituição


Implementamos um novo campo para com o id do pagamento na instituição:` endToEndId` .


Esse campo será retornado na response de pagamentos agendados **concluídos** e também no evento de webhook` scheduled_payment/completed` , conforme exemplo
` { "paymentRequestId": "a0ed730e-5a44-491f-9a32-aa295c399fdf", "event": "scheduled_payment/completed", "eventId": "1d5d1aab-fc1a-40d4-aeae-15e7b9c11a10", "endToEndId":"E44471172202505211500U0d9ffa12345", "scheduledPaymentId": "65c6f9cd-e69f-46cd-8103-80b42dd61bfd" }`


---


#


✨ Melhorias


##


🎲 Dados


###


Descontinuação de conectores diretos (não-regulados)


- O conector **Genial Investimentos** está sendo descontinuado devido a medidas de segurança implementadas pela instituição.


- Este conector **não está disponível** no Open Finance regulado, dessa forma, não teremos mais acesso a conexões da instituição Genial até que se tornem participantes do Open Finance.


- O conector **Sicoob (PF & PJ)** está sendo descontinuado devido a medidas de segurança implementadas pela instituição.


- Este conector **está disponível** no Open Finance regulado, logo, para continuar tendo acesso ao compartilhamento de dados nessa instituição, será necessária uma migração para o conector regulado.


*Dúvidas?* Fale com nossa equipe!__


---


##


💳 Pagamentos


###


Novos filtros para pagamentos


Agora é possível realizar buscas mais refinadas utilizando múltiplos filtros e filtros simples para os endpoints de **Recipients, Customers e Requests** .


- Endpoint *Payment Recipient*
Novos filtros disponíveis:


- ` pixKey`
- ` name`


📚[Documentação](https://docs.pluggy.ai/reference/payment-recipients-list)


- Endpoint *Payment Customers* :
Novos filtros disponíveis


- ` name`
- ` email`
- ` cpf`
- ` cnpj`


📚[Documentação](https://docs.pluggy.ai/reference/payment-customers-list)


- Endpoint *Payment Request* :
Novos filtros disponíveis


- ` from` e` to` : intervalo de datas baseado no campo` createdAt`
- ` customer`
- ` pixKey`


📚[Documentação](https://docs.pluggy.ai/reference/payment-requests-list)


###


⚠️ Importante:


- Os filtros aceitam **buscas parciais** e sem **diferenciação de maiúsculas/minúsculas** , usando a cláusula` ILIKE` , ou seja, uma busca por` john` retornará tanto` John Doe` quanto` john smith` ;
- Os filtros podem ser combinados em uma única requisição para refinar os resultados.


---


#


📈 Bugs


###


🎲 Dados


- Itaú PJ: foram implementadas melhorias para:


- estabilização do conector;
- transações de cartão de crédito.


- Santander PJ: foram implementadas melhorias para:


- estabilização do conector;
- erros com data da transação.


- Bradesco PJ: corrigida lentidão para atualização de contas.
