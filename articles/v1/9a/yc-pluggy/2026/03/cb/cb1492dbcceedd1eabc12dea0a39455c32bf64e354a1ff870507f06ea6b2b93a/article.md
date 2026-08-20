---
schema_version: "1.0.0"
document_id: "cb1492dbcceedd1eabc12dea0a39455c32bf64e354a1ff870507f06ea6b2b93a"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-388d271f0e5d"
canonical_url: "https://pluggy.ai/blog/pix-scheduler-automatizar-cobranca-recorrente-via-pix"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-22T09:30:39.558676+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:ed9500220e59d39dea80a50cf8c6ecaa23a4216e755af4a6306453f0ff28be93"
---

# Pix Scheduler: automação de cobrança recorrente via Pix com ITP

## O que é o Pix Scheduler e como funciona


O Pix Scheduler é uma funcionalidade que automatiza o agendamento de cobranças recorrentes através do Pix Automático utilizando ITP (Iniciação de Transação de Pagamento).


-


Ative schedulerConfiguration.enabled: true na criação do Payment Request


-


Após autorização do cliente, o sistema agenda automaticamente os próximos ciclos


-


O agendamento respeita a janela permitida (D+2 a D+10)


-


Não é necessário chamar manualmente o endpoint /automatic-pix/schedule a cada período


## Por que isso importa: do operacional manual à infraestrutura programável


Muitas empresas ainda operacionalizam cobranças recorrentes de forma manual, dependendo de cron jobs monitorados, verificações mensais, ajustes manuais em caso de falha e acompanhamento constante do ciclo de cobrança.


### Antes do Pix Scheduler


Execução manual do endpoint de schedule a cada período, mesmo com cliente já autorizado.


### Com o Pix Scheduler


A recorrência torna-se parte da infraestrutura do produto, oferecendo:


-


Agendamento automático via API


-


Redução do risco operacional


-


Aumento da previsibilidade de receita


-


Times focados em tarefas de maior valor agregado


## O mínimo esperado para crescer com previsibilidade e eficiência


Automatizar cobrança recorrente via Pix representa uma decisão estratégica. Tratar recorrência como infraestrutura programável fortalece o modelo de negócio e reduz fragilidade operacional.


A funcionalidade já está disponível para implementação através da documentação oficial.
