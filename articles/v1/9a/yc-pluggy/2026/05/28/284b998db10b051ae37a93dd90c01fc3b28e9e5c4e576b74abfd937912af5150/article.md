---
schema_version: "1.0.0"
document_id: "284b998db10b051ae37a93dd90c01fc3b28e9e5c4e576b74abfd937912af5150"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/pix-automatico-api-erp-guia-tecnico"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:38:50.569956+00:00"
fetched_at: "2026-07-28T22:15:28.620730+00:00"
content_hash: "sha256:21903066c30b89a03913f7c08b133693f59c5ca7dd6c2433ff8f676742d5d042"
---

# Como implementar Pix Automático no seu ERP: guia técnico com a API da Pluggy

Implementar Pix Automático no seu ERP via API leva menos tempo do que parece — e resolve de uma vez o problema de cobranças recorrentes que ainda dependem de boleto ou débito automático.


## O que você vai implementar neste guia


-


Setup da API de Pix Automático da Pluggy


-


Criação de mandatos de cobrança recorrente (valor fixo e variável)


-


Fluxo de consentimento e tratamento de webhooks


-


Agendamento automático e retry em caso de falha


-


Testes no sandbox antes do deploy em produção


## Arquitetura da integração


Antes de escrever código, entenda o fluxo de dados: ERP → API da Pluggy → Banco Central (SPI) → banco do cliente. O seu ERP chama a API da Pluggy; ela cuida de tudo entre você e o banco do cliente: autenticação no SPI, gestão de mandato, retries e notificações de status via webhook.


São dois caminhos de integração. Payment Gateway (hospedado): a Pluggy redireciona o usuário para uma página de autorização que ela gerencia; menos código. API direta: você constrói o fluxo de consentimento dentro da sua interface; mais controle de UX. Para ERPs que querem manter o usuário dentro da plataforma de ponta a ponta, a API direta costuma ser preferida.


## Setup inicial: credenciais, recebedor e webhooks


Você vai precisar de: credenciais de acesso à API (clientId + clientSecret), um PaymentRecipient criado para receber os pagamentos, uma callbackUrl configurada para receber webhooks de status, e o sandbox ativo para testar antes da produção.


### Criando o PaymentRecipient


O recebedor é o destino dos pagamentos. Crie uma vez e reaproveite em todos os mandatos. A resposta inclui o recipientId — salve esse valor, ele é obrigatório em todo mandato.


```text
POST https://api.pluggy.ai/payments/recipients
Authorization: Bearer {your_api_key}
Content-Type: application/json


{
"name": "Empresa XYZ Ltda",
"taxNumber": "12.345.678/0001-99",
"paymentInstitutionId": "60746948-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
"account": {
"branch": "0001",
"number": "123456",
"type": "CHECKING"
}
}
```


### Configurando webhooks


Defina a callbackUrl que recebe as notificações de status de cada cobrança. A URL precisa ser HTTPS e responder com status 2xx. Repita para os três eventos principais: payment *request/updated (status do mandato), automatic* pix *payment/created (cobrança criada no ciclo) e automatic* pix_payment/completed (cobrança liquidada).


```text
POST https://api.pluggy.ai/webhooks
Authorization: Bearer {your_api_key}
Content-Type: application/json


{
"event": "payment_request/updated",
"url": "https://seuapp.com.br/webhooks/pluggy",
"clientId": "{seu_client_id}"
}
```


## Como criar um mandato de cobrança


Esse é o núcleo da integração. O mandato define quem paga, quanto, com que frequência e por quanto tempo. Endpoint: POST /payments/requests/automatic-pix. Os valores são em centavos (R$ 99 = 9900). Atenção a esse detalhe pra não cobrar errado.


### Exemplo 1: valor fixo (assinatura mensal de R$ 99)


```text
{
"description": "Assinatura mensal ERP XYZ",
"recipientId": "recipient_abc123",
"interval": "MONTHLY",
"startDate": "2026-02-01",
"fixedAmount": 9900,
"callbackUrls": {
"success": "https://seuapp.com.br/pagamentos/sucesso",
"error": "https://seuapp.com.br/pagamentos/erro",
"pending": "https://seuapp.com.br/pagamentos/pendente"
},
"firstPayment": {
"date": "2026-02-01",
"amount": 9900,
"description": "Primeira mensalidade ERP XYZ"
}
}
```


Campos obrigatórios: recipientId; interval (WEEKLY, MONTHLY, QUARTERLY, SEMESTER ou YEARLY); startDate (ISO 8601); e fixedAmount ou os campos de valor variável.


### Exemplo 2: valor variável (entre R$ 50 e R$ 300)


```text
{
"description": "Cobrança mensal por consumo",
"recipientId": "recipient_abc123",
"interval": "MONTHLY",
"startDate": "2026-02-01",
"minimumVariableAmount": 5000,
"maximumVariableAmount": 30000,
"firstPayment": {
"date": "2026-02-01",
"amount": 12000,
"description": "Cobrança de fevereiro por consumo"
}
}
```


## Fluxo de autorização e webhooks


Depois de criar o mandato, a API retorna um campo paymentUrl. Redirecione o usuário para essa URL. Ele autentica no app do banco e autoriza os débitos automáticos — o processo leva menos de dois minutos. Se não concluir em 60 minutos, o mandato expira com status CONSENT_REJECTED.


```text
app.post('/webhooks/pluggy', (req, res) => {
const { event, data } = req.body;
if (event === 'payment_request/updated') {
switch (data.status) {
case 'ACCEPTED':
activateSubscription(data.paymentRequestId);
break;
case 'CONSENT_REJECTED':
handleConsentRejection(data.paymentRequestId);
break;
case 'COMPLETED':
deactivateSubscription(data.paymentRequestId);
break;
}
}
if (event === 'automatic_pix_payment/completed') {
markInvoicePaid(data.paymentId, data.amount);
}
res.status(200).send('OK');
});
```


## Agendamento e retries automáticos


Para controle granular de cada ciclo, use POST /payments/requests/{id}/automatic-pix/schedule. A data agendada precisa cair entre D+2 e D+10 do vencimento configurado, e só é permitido 1 pagamento por intervalo. Para listar os agendamentos: GET /payments/requests/{id}/automatic-pix/schedules.


Para volume alto, o Scheduler (beta) automatiza o agendamento ciclo a ciclo — habilite com schedulerConfiguration.enabled = true no mandato. Cobranças que falham por saldo insuficiente podem ser repetidas automaticamente: configure automaticRetriesConfiguration.retryDays com um array de 1 a 3 inteiros de 1 a 7 (dias da semana). Cada retry gera um novo webhook automatic *pix* payment/created.


```text
{
"schedulerConfiguration": { "enabled": true },
"automaticRetriesConfiguration": { "retryDays": [3] }
}
```


## Testando no sandbox


O sandbox replica o comportamento de produção sem mover dinheiro real. Você pode criar mandatos e simular autorizações, disparar webhooks de teste com status diferentes e simular os fluxos de CONSENT_REJECTED e retry. Use as credenciais de sandbox disponíveis no dashboard.


## Perguntas frequentes


Pix Automático funciona para cobrança B2B? Sim, mas o recebedor precisa ter CNPJ. Um recebedor criado com CPF retorna erro "AUTOMATIC *PIX* PAYMENT *RECIPIENT* NOT_CNPJ" na criação do mandato.


Quanto tempo leva a integração? Com credenciais de sandbox e o endpoint de webhook configurado, os primeiros testes rodam em menos de um dia. A integração completa, com todos os handlers de status e o fluxo de consentimento na UI, costuma fechar em uma sprint.


A Pluggy é regulada para iniciação de pagamento? Sim. A Pluggy é ITP autorizada pelo Banco Central, e toda a operação de Pix Automático via API opera dentro do arcabouço regulatório do Open Finance.


Documentação


Referência completa de Pix Automático, getting started, scheduler e retries em docs.pluggy.ai. Crie sua conta no sandbox e dispare o primeiro mandato de teste em menos de 30 minutos.


[Ver changelog completo](https://docs.pluggy.ai/)
