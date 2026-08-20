---
schema_version: "1.0.0"
document_id: "79d74836af3f37dc045f5e30d49d9f9026ef1373c2b8460e308f18220d46cf0e"
company_key: "yc-pluggy"
company: "Pluggy"
source_id: "yc-pluggy-news-import-4bbcede04bdb"
canonical_url: "https://docs.pluggy.ai/changelog/atualiza%C3%A7%C3%B5es-de-produto-fevereiro-2026"
published_at: null
first_seen_at: "2026-07-22T09:30:43.038391+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:87d7823c329c0cf6d7d74ad5b72e3c409aad0d30b9e6d5de2097748ef01e7517"
---

# Atualizações de Produto | Fevereiro 2026

[Back to All](https://docs.pluggy.ai/changelog)


> 📘
>
>
> ##
>
>
> Confira as principais novidades e melhorias da Pluggy em fevereiro de 2026.
>
>
> **Destaques de fevereiro**
>
>
> - Lançamento do novo Dashboard Pluggy, com ambientes separados, onboarding guiado e gestão de equipe.
> - Novas capacidades de Pix recorrente via PixAuto, incluindo agendamento e retentativa automática.
> - Expansão de conectores com Conta Bemol PF e Inter PJ para MEI.
> - Melhorias em webhooks, incluindo retentativa via dashboard e autenticação personalizada.
> - Diversas correções e melhorias de estabilidade em conectores e sincronização de dados.


---


##


🖥️ Novo Dashboard


Lançamos o **novo Dashboard Pluggy** , completamente redesenhado para facilitar a gestão da sua integração.


→ **Ambientes separados** — agora você pode alternar facilmente entre Development e Production direto no menu lateral. Suas aplicações, eventos e filtros se ajustam automaticamente ao ambiente selecionado.


→ **Onboarding guiado para novos usuários** — ao criar sua conta, você será guiado por 4 etapas práticas: gerar seu primeiro connect token, integrar o widget e configurar webhooks — tudo direto no dashboard.


→ **Tour interativo** — um tour passo a passo apresenta as principais áreas do dashboard para você começar a usar rapidamente.


→ **Visão geral completa** — acompanhe suas conexões diárias, status de execuções e distribuição por instituição com gráficos detalhados e filtros por conector e período.


→ **Due Diligence digital** — preencha e envie seu formulário de compliance diretamente pelo dashboard, com upload de documentos e acompanhamento de status.


→ **Personalização do widget** — visualize as alterações de marca em tempo real, faça upload do seu logo com drag-and-drop e veja um indicador visual quando houver alterações pendentes.


→ **Gestão de equipe** — convide colaboradores por email, defina papéis e gerencie convites pendentes.


→ **Retentativa de webhooks em lote** — selecione um intervalo de tempo e reenvie webhooks diretamente pelo dashboard.


→ **Alertas de conectores** — receba avisos automáticos quando conectores utilizados na sua integração estiverem offline ou instáveis.


---


##


💳 Pagamentos / PixAuto


→ **Agendamento de pagamentos Pix** — agora você pode agendar pagamentos Pix recorrentes via PixAuto.


→ **Retentativa automática** — pagamentos Pix que falharem serão retentados automaticamente, sem necessidade de intervenção manual.


→ **Identificação de primeiro pagamento** — a API agora indica quando um pagamento é o primeiro de uma série, facilitando o controle no seu sistema.


→ **Link de pagamento revogado** — links de pagamento são desativados automaticamente quando o consentimento do usuário é revogado, garantindo segurança.


---


##


📡 Novos Conectores


→ **Conta Bemol Física** — conecte contas de pessoa física do Conta Bemol via Open Finance.


→ **Inter PJ para MEI** — microempreendedores individuais agora são atendidos pelo conector regulado do Inter PJ.


---


##


🔔 Webhooks


→ **Headers de autenticação personalizados** — seus webhooks agora recebem os headers de autenticação que você configurar, facilitando a validação no seu servidor.


→ **Retentativa por intervalo de tempo** — reenvie webhooks de um período específico caso tenha perdido algum evento.


→ **Retentativa pelo dashboard** — reenvie webhooks diretamente pela interface, sem precisar usar a API.


→ **Desativação automática de URLs inativas** — URLs de webhook que não recebem eventos há um período prolongado são desativadas automaticamente para evitar chamadas desnecessárias.


→ **Limite de webhooks por evento** — agora há um limite configurável de webhooks por tipo de evento para melhor organização.


---


##


🚀 Novas Features


→ **Subtipo de conta (Open Finance)** — o campo` accountSubtype` agora é retornado na resposta da API, oferecendo mais detalhes sobre o tipo de conta conectada.


→ **Filtro por data de criação de transações** — filtre transações pela data em que foram detectadas pela Pluggy, útil para identificar novas transações desde a última sincronização.


→ **Prevenção de conexões duplicadas** — durante a seleção de conta com MFA, contas já conectadas são bloqueadas para evitar duplicidade.


→ **Validação de CNPJ (Bradesco PJ)** — o CNPJ informado no formulário é validado contra a instituição, evitando erros de conexão.


---


##


🐛 Correções


→ **Datas de transações** — corrigidas datas inválidas em transações de cartão de crédito (Caixa) e boletos (Bradesco PJ).


→ **Extrato PDF (Itaú PJ)** — download de extrato em PDF restaurado para contas Itaú PJ.


→ **Conexão Itaú PJ** — resolvido erro de conexão causado por expiração de permissões.


→ **Sincronização histórica** — corrigido loop infinito que podia ocorrer durante a sincronização de dados históricos via Open Finance.


→ **Transações duplicadas** — eliminada duplicação de transações com mesmo identificador em conectores Banrisul e Banco do Brasil.


→ **Entrega de webhooks** — corrigidos cenários em que webhooks de transação atualizada, exclusão de item e criação de conta não eram entregues corretamente (afetava Inter MEI e Santander).


→ **Correções de dados** — ajustes pontuais em transações de diversos conectores: Itaú, Bradesco, Santander, Banrisul, Banco do Brasil, Nubank, Sicredi, Caixa, BTG e Inter.


---


##


📚 Documentação


→ **Documentação para LLMs** — nossa documentação agora está otimizada para uso com modelos de linguagem (IA), facilitando integrações assistidas por IA.


→ **Smart Transfers** — documentação de Smart Transfers atualizada com os novos recursos.


→ **SDKs** atualizado para refletir as últimas funcionalidades da API.
