---
schema_version: "1.0.0"
document_id: "51f0fb81a60b35f457a7601ae54c430aa4602935bcbcf284f7bb15d8c1a5a6ac"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-news-import-7663787d24e6"
canonical_url: "https://pt.semrush.com/blog/google-search-console-errors/"
published_at: "2025-05-13T16:59:09+00:00"
first_seen_at: "2026-07-22T16:50:14.188504+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:d098b968c2c5298957c98821bf22a99e77cd6f4453bccde679952729571fb080"
---

# Erros do Google Search Console: como identificá-los e corrigi-los

Traduzimos este artigo do inglês.[Clique aqui](https://www.semrush.com/blog/google-search-console-errors/) para ler o artigo original. Se você notar algum problema com o conteúdo, não hesite em entrar em contato conosco pelo e-mailreport-osteam@semrush.com .


O Google Search Console é um serviço online que permite monitorar e gerenciar a presença do seu site nos resultados de busca do Google.


Isso também ajuda a identificar erros potencialmente impactantes no site. Aqui está o que você precisa saber sobre esses erros e como corrigi-los.


## Entendendo os erros do Google Search Console


Se o Googlebot encontrar problemas ao rastrear seu site, esses problemas poderão ser sinalizados como erros no Google Search Console.


É importante analisar essas questões.


Por que?


Por exemplo, erros não resolvidos podem impedir que o Google indexe seu site corretamente. Páginas não indexadas não aparecerão nos resultados da pesquisa.


Erros não resolvidos no site também podem afetar negativamente a experiência do usuário. Isso é ruim para os visitantes. E é um fator de classificação fundamental para o Google, portanto, um desempenho ruim nessa área também pode fazer com que o Google classifique você em uma posição inferior nos resultados de pesquisa.


Vamos ver como corrigir erros no Google Search Console.


## Tipos de erros do Google Search Console e como corrigi-los


### Erros de código de status


Os erros de código de status no Google Search Console referem-se aos códigos de status HTTP retornados quando o Googlebot tenta rastrear suas páginas da web.


Os códigos de status HTTP são retransmitidos sempre que um site é carregado. Quando um usuário digita um URL ou um bot visita o site, uma solicitação é enviada ao servidor desse site. O servidor responde com um código de status HTTP que informa ao navegador — ou bot — mais informações sobre aquela página.


Os códigos de status HTTP são números de três dígitos. Por exemplo, “200”, “301” ou “404”.


Diferentes códigos de status transmitem informações diferentes sobre o sucesso ou falha de uma solicitação.


Por exemplo, o código de status “200” indica que está tudo bem e a página pode ser acessada conforme o esperado.


No entanto, alguns códigos de status indicam que houve problemas ao acessar a página da web. Esses erros aparecem no Google Search Console. Porque podem afetar negativamente a capacidade dos rastreadores e dos usuários de acessarem seu site.


Para verificar se há algum erro de código de status, abra o[Google Search Console](https://search.google.com/search-console/about) e clique em “ **Páginas** ” no menu à esquerda.


Você verá uma análise das páginas do seu site, classificando-as como "indexadas" ou "não indexadas".


É possível que algumas páginas não tenham sido indexadas devido a um erro no código de status.


Para verificar se é esse o caso, role até o final do relatório. Você verá uma tabela com o título "Por que as páginas não são indexadas".


Caso ocorram erros de status, você verá uma das seguintes opções em “Motivo”:


- Erro do servidor (5xx)
- Não encontrado (404)
- Solicitação não autorizada (401)
- Soft 404
- Erro de redirecionamento (3xx)


Vamos analisar o significado dos diferentes códigos de erro de status e como corrigi-los.


#### Erros do servidor (5xx)


Um código de status de três números começando com “5” indica um erro no servidor. Isso indica um problema com o servidor que hospeda seu site.


Veja como um erro de servidor aparece no Google Search Console:


As causas comuns de um status 5xx incluem:


- **Erros de programação** : Bugs ou erros de codificação nos scripts do lado do servidor (por exemplo, PHP, Python, Ruby) que alimentam seu site
- **Problemas de configuração do servidor** : Configurações incorretas nas definições do servidor ou no software do servidor web (ex.: Apache, Nginx)
- **Esgotamento de recursos** : Se o servidor ficar sem recursos (como memória ou poder de processamento), ele poderá ter dificuldades para lidar com as solicitações, levando a erros internos do servidor.
- **Problemas no banco de dados** : Problemas com o banco de dados do qual seu site depende, como problemas de conexão ou erros no servidor de banco de dados, podem causar um erro 5xx


#### Como corrigir um erro de servidor (5xx) no Google Search Console


- **Analisar alterações recentes.** Se o erro começou a ocorrer após atualizações ou alterações recentes no seu site, considere reverter essas alterações para ver se o problema é resolvido.
- **Contate seu provedor de hospedagem.** Se você não tiver certeza de como resolver o erro do servidor ou se ele estiver relacionado à infraestrutura do servidor, entre em contato com seu provedor de hospedagem para obter assistência. Eles podem ser capazes de identificar e corrigir o problema.
- **Testar recursos do servidor.** Certifique-se de que seu servidor tenha recursos suficientes (CPU, memória, espaço em disco) para lidar com o tráfego e as solicitações do seu site. Considere atualizar seu plano de hospedagem, se necessário.


Depois de identificar e resolver a causa subjacente do erro 5xx do servidor, use o Google Search Console para solicitar uma nova indexação das páginas afetadas.


Para isso, basta inserir o URL afetado na caixa de pesquisa na parte superior do Google Search Console:


Em seguida, clique em “ **SOLICITAR INDEXAÇÃO** ”:


Isso instrui o Googlebot a revisitar as páginas e atualizar seu índice com as informações corrigidas.


#### Não encontrado (404) Erros


O erro "404 Não encontrado" é um código de resposta HTTP padrão retornado quando um servidor não consegue encontrar o conteúdo associado à URL solicitada.


No contexto do Google Search Console, isso significa que o Googlebot tentou rastrear uma página do seu site, mas o servidor retornou um erro 404 porque não conseguiu encontrar o conteúdo da página.


Aqui estão alguns motivos comuns pelos quais um erro 404 pode ocorrer:


- **Exclusão ou remoção de página** : Se você excluiu intencionalmente uma página ou a removeu do seu site e o Googlebot tentar rastreá-la, o servidor retornará um erro 404.
- **Alterações de URL sem redirecionamentos adequados** : Se você alterou a estrutura de URLs do seu site sem implementar redirecionamentos adequados (por exemplo, redirecionamentos 301), as URLs antigas podem resultar em erros 404.
- **Erro de digitação ou URL digitada incorretamente** : É possível que tenha ocorrido um erro de digitação ou um erro na URL fornecida ao Googlebot. Certifique-se de que os URLs no seu mapa do site e os links internos estejam corretos e direcionem para páginas existentes.


#### Como encontrar erros 404 no Google Search Console


Abra o Google Search Console e clique no relatório “ **Páginas** ”.


Deslize a tela para baixo para ver "Por que as páginas não estão indexadas". Aqui, você poderá ver se alguma página apresenta erro 404.


#### Como corrigir um erro 404 no Google Search Console


- **Implementar redirecionamentos 301.**
- **Atualizar links internos.** Se houver links em seu site para páginas excluídas, atualize esses links para apontar para URLs válidos. Você pode encontrar esses links usando a[Auditoria de Site](https://www.semrush.com/siteaudit///) do Semrush.
- **Verifique os links externos.** Se sites externos estiverem criando links para páginas inexistentes em seu site, considere entrar em contato com esses sites e solicitar que atualizem seus links.
- **Enviar um mapa do site.** Certifique-se de que o mapa do site esteja atualizado e reflita com precisão a estrutura atual do seu site.


#### Erros de solicitação não autorizada (401)


Um erro com o código de status HTTP "401" no Google Search Console indica que uma solicitação não foi autorizada.


Fonte da imagem:[Onely](https://www.onely.com/blog/how-to-fix-blocked-due-to-unauthorized-request-401/)


O erro 401 ocorre quando:


- A página contém conteúdo protegido por senha, o que significa que o Googlebot e outros rastreadores não conseguem acessá-la.
- Existem bloqueios de IP ou restrições de acesso que impedem que os endereços IP do Googlebot acessem a página.
- Problemas de configuração específicos do rastreador foram configurados acidentalmente.


#### Como corrigir um erro 401 (Não autorizado) no Google Search Console


- **Verifique as configurações de autenticação.** Se a página exigir autenticação (por exemplo, se for necessário fazer login para visualizar o conteúdo), verifique as configurações de autenticação para garantir que estejam configuradas corretamente.
- **Testar acesso.** Teste manualmente o acesso às páginas afetadas, tentando acessá-las por meio de um navegador da web ou usando ferramentas como o recurso "Buscar como o Google" no Google Search Console. Isso pode ajudar a identificar quaisquer problemas de acesso.
- **Analise as restrições de acesso.** Verifique se há bloqueios de IP ou restrições de acesso em seu servidor ou site. Certifique-se de que os endereços IP do Googlebot não estejam bloqueados e que as permissões necessárias para a indexação estejam em vigor.
- **Verificar acesso do agente do usuário.** Confirme se a configuração do seu site permite o acesso a agentes de usuário de mecanismos de busca comuns, incluindo o Googlebot. Caso existam regras que limitem o acesso a determinados agentes de usuário, certifique-se de que estejam configuradas corretamente.


Após resolver o problema de acesso não autorizado, use o Google Search Console para solicitar uma nova indexação das páginas afetadas.


#### Erros 404 suaves


Os erros Soft 404 no Google Search Console ocorrem quando uma página parece normal (retorna um código de status 200), mas o conteúdo ou certos sinais na página indicam que ela deve ser tratada como se não existisse.


Isso pode ser confuso para mecanismos de busca como o Google. E isso pode afetar a forma como suas páginas são indexadas e exibidas nos resultados de pesquisa. O Google Search Console os sinaliza como erros.


Cenários comuns que levam a erros 404 suaves incluem:


- **Páginas vazias** : Páginas que estão vazias ou contêm muito pouco conteúdo podem gerar erros 404 suaves. Embora o servidor retorne um código de status "200 OK", a falta de conteúdo sugere que a página não está fornecendo informações relevantes aos usuários.
- **Páginas redirecionadas** : Se uma página for redirecionada para outro URL, mas o conteúdo no URL de destino for escasso ou irrelevante, o Google poderá interpretá-lo como um erro 404 suave.
- **Páginas de erro personalizadas** : Se o seu site tiver uma página de erro personalizada que exibe uma mensagem de erro ao usuário — mesmo que ela tenha um código de status “200 OK” — isso pode acionar erros 404 temporários.


#### Como corrigir um erro 404 temporário no Google Search Console


- **Revisar o conteúdo da página.** Examine o conteúdo das páginas sinalizadas como erros 404 temporários. Garantir que forneçam informações relevantes aos usuários. Se o conteúdo for insuficiente, considere melhorá-lo ou redirecionar os usuários para uma página mais relevante.
- **Verificar redirecionamentos.** Se uma página estiver sendo redirecionada, certifique-se de que o URL de destino contenha conteúdo relevante. Evite redirecionar os usuários para páginas que não estejam relacionadas ao conteúdo da página original.
- **Verificar páginas de erro personalizadas.** Se o seu site utiliza páginas de erro personalizadas, certifique-se de que elas forneçam informações úteis e não sejam confundidas com conteúdo vazio ou irrelevante.


### Erros de redirecionamento


Redirecionamentos enviam usuários e rastreadores de mecanismos de busca de um URL para outro.


Os erros de redirecionamento ocorrem quando um redirecionamento falha por algum motivo. Esses erros podem afetar negativamente a forma como mecanismos de busca como o Google rastreiam, indexam e classificam seu site.


Os erros de redirecionamento mais comuns incluem:


- **Cadeias de redirecionamento** : Isso ocorre quando um URL redireciona para outro e, em seguida, esse segundo URL redireciona para um terceiro, formando uma sequência de redirecionamentos. Cadeias de redirecionamento podem tornar o carregamento de páginas mais lento e afetar negativamente a experiência do usuário e a eficiência da indexação pelos mecanismos de busca.
- **Loops de redirecionamento** : Isso acontece quando o URL de destino final redireciona de volta para um URL anterior na cadeia. Isso pode causar um loop infinito de redirecionamentos, impedindo o carregamento da página e levando a erros de rastreamento.
- **Redirecionamentos incorretos** : Se uma página for redirecionada para um URL incorreto ou irrelevante, isso pode levar a um erro de redirecionamento. Os redirecionamentos devem ser configurados corretamente e direcionar usuários e mecanismos de busca para um URL de destino relevante.


### Como corrigir um erro de redirecionamento no Google Search Console


Identifique quaisquer erros de redirecionamento no Google Search Console visitando o relatório “ **Páginas** ”. Deslize a tela para baixo para ver "Por que as páginas não estão indexadas".


- **Analisar cadeias de redirecionamento.** Caso sejam identificadas cadeias de redirecionamento, revise-as e simplifique-as. Idealmente, um redirecionamento deve levar diretamente ao destino final, sem etapas intermediárias desnecessárias.
- **Corrigir loops de redirecionamento.** Caso sejam detectados loops de redirecionamento, identifique a origem do loop e corrija as configurações de redirecionamento para interrompê-lo. Certifique-se de que o URL de destino final esteja correto.
- **Verificar destinos de redirecionamento.** Verifique se os URLs para os quais as páginas são redirecionadas estão corretos e são relevantes. Redirecionamentos incorretos podem confundir usuários e mecanismos de busca.
- **Atualizar links internos.** Se você alterou URLs e implementou redirecionamentos, atualize os links internos do seu site para apontarem diretamente para as novas URLs, reduzindo a necessidade de redirecionamentos.
- **Use códigos de status de redirecionamento adequados.** Ao implementar redirecionamentos, utilize os códigos de status HTTP apropriados. Por exemplo, um redirecionamento 301 indica uma mudança permanente, enquanto um redirecionamento 302 sinaliza uma mudança temporária.


## Problemas de rastreamento e indexação


Problemas de rastreamento e indexação ocorrem quando os rastreadores do Google não conseguem acessar uma página que deveriam conseguir acessar. Ou quando algo os impede de adicionar uma página ao seu índice.


Você pode verificar esses problemas no relatório “ **Páginas** ”.


Abra o relatório e role para baixo até a tabela "Por que as páginas não são indexadas".


Você poderá encontrar os seguintes problemas.


### Bloqueado pelo Robots.txt


Se uma página estiver bloqueada pelo arquivo robots.txt, isso pode ser um problema. Mas apenas se você não quiser que essa página seja bloqueada. Algumas páginas estão bloqueadas intencionalmente.


O arquivo robots.txt fica localizado no seu site e contém instruções para os rastreadores dos mecanismos de busca. Geralmente é a primeira página que eles visitam no seu site.


Você pode incluir instruções no arquivo robots.txt para instruir os robôs de busca a não rastrearem determinadas páginas. Por exemplo, páginas administrativas que não são acessíveis sem login.


Você faz isso usando uma diretiva "disallow".


No entanto, é possível que uma página seja bloqueada pelo robots.txt por engano.


No Google Search Console, se você vir uma página que diz "bloqueada pelo robots.txt", mas quiser que o Google rastreie essa página, você pode editar o arquivo robots.txt. Em seguida, use o Google Search Console para solicitar uma nova indexação das páginas afetadas.


### Como corrigir páginas bloqueadas pelo robots.txt


Você pode revisar e corrigir quaisquer problemas com seu arquivo robots.txt usando a ferramenta[Auditoria do Site](https://www.semrush.com/siteaudit///) .


Abra a ferramenta e clique em “ **+ Criar projeto** .”


Insira o domínio do seu site. Se quiser, você também pode dar um nome ao projeto.


Clique no botão “ **Criar projeto** ”.


Configure quaisquer definições adicionais que desejar para a auditoria do site. Em seguida, clique em “ **Iniciar auditoria do site** .”


Assim que a auditoria estiver concluída, vá para a aba " **Problemas** " e procure por "robots.txt".


Clique em qualquer problema relacionado ao seu arquivo robots.txt, como "O arquivo robots.txt contém erros de formatação".


Será exibida uma lista das linhas inválidas no arquivo.


Clique em " **Por que e como corrigir** " para obter instruções específicas sobre como resolver o erro.


## Problemas de experiência do usuário e usabilidade


Problemas em sites que causam experiências ruins para o usuário são preocupantes por dois motivos.


Em primeiro lugar, você não quer que os visitantes do seu site tenham experiências ruins. Isso gera desconfiança na sua marca. E isso diminui as chances de essa pessoa interagir com seu conteúdo novamente.


Experiências ruins do usuário também são prejudiciais porque experiências positivas são um fator importante de classificação na Busca do Google. Páginas com experiências de usuário ruins não terão um bom desempenho nos resultados de busca.


### Problemas com as principais métricas da Web


As Core Web Vitals são um conjunto de métricas centradas no usuário que medem o desempenho de carregamento, a interatividade e a estabilidade visual de uma página.


Essencialmente, as Core Web Vitals medem a velocidade e a facilidade de uso de um site.


As três principais métricas são:


- **Maior Renderização de Conteúdo (LCP)** : O tempo que o maior elemento de conteúdo (como uma imagem ou um bloco de texto) leva para se tornar visível na tela do usuário.
- **Interação para a Próxima Renderização (INP)** : Quão bem a página da web responde às interações do usuário
- **Deslocamento de layout cumulativo (CLS)** : Quantas vezes a página se desloca durante o carregamento


O Google Search Console avisa quando você tem problemas de desempenho relacionados às Core Web Vitals.


Você pode identificar esses problemas usando a ferramenta[Auditoria do Site](https://www.semrush.com/siteaudit//) . E receba dicas sobre como consertá-los.


### Como corrigir problemas com as Core Web Vitals


Abrir[Auditoria do Site](https://www.semrush.com/siteaudit//) . Você pode escolher um projeto existente ou clicar em “ **+ Criar projeto** .”


Configure as definições e clique em “ **Iniciar auditoria do site** ”.


Em poucos minutos, você verá seus resultados.


No relatório "Visão geral", você verá um widget à direita chamado "Indicadores vitais da Web principais". Clique em “ **Ver detalhes** .”


Você poderá ver o desempenho do seu site em relação às métricas Core Web Vitals para 10 páginas.


Desça até "Métricas" e você poderá ver o desempenho detalhado nas três principais métricas da Web.


Abaixo de cada um, há uma lista intitulada "Principais melhorias" com sugestões para melhorar seu desempenho nessa área.


### Erros de HTTPS no Google Search Console


O Google prioriza sites com certificado SSL. Este é um certificado de segurança que criptografa os dados enviados entre um site e um navegador.


Depois de obter um certificado SSL, todas as páginas do seu site devem começar com “https://”.


Páginas que começam com “http://” podem indicar problemas de segurança para os mecanismos de busca. Portanto, o melhor é corrigi-los assim que surgirem.


Abra o Google Search Console e clique em “HTTPS” no menu à esquerda.


A tabela mostrará se você tem alguma URL não HTTPS em seu site.


### Como corrigir erros de HTTPS no Google Search Console


Se você encontrar erros de HTTPS, role a página para baixo no Google Search Console. Você verá uma tabela chamada "Por que as páginas não estão sendo servidas via HTTPS".


Clique na linha do problema para ver quais URLs foram afetadas.


Depois de visualizar a lista de URLs, você pode remover todos os links para essas páginas do seu site e solicitar ao Google Search Console que rastreie seu site novamente.


## Ações manuais no Google Search Console


As ações manuais no Google Search Console referem-se a penalidades impostas a um site por revisores humanos que trabalham no Google. Quando esses revisores identificam violações das diretrizes do Google, eles podem tomar medidas manuais para penalizar o site.


As ações manuais são diferentes das penalidades algorítmicas, que são aplicadas automaticamente pelos algoritmos do Google.


Os motivos mais comuns para ações manuais incluem:


- **Links não naturais** : Se um site estiver envolvido em esquemas de links, tiver comprado links ou participar de trocas de links que violem as diretrizes do Google, poderá receber uma ação manual.
- **Conteúdo insuficiente ou duplicado** : Sites com conteúdo de baixa qualidade ou duplicado podem sofrer sanções manuais por violarem as diretrizes de qualidade de conteúdo.
- **Ocultação ou redirecionamentos furtivos** : Se um site exibir conteúdo diferente para usuários e mecanismos de busca ou usar redirecionamentos enganosos, poderá receber uma ação manual.
- **Marcação estruturada de spam** : O uso incorreto de marcação de dados estruturados (schema.org) para manipular resultados de pesquisa pode levar a ações manuais
- **Spam gerado pelo usuário** : Sites que permitem que os usuários gerem conteúdo (comentários, fóruns) podem sofrer sanções manuais se não moderarem e controlarem o spam de forma eficaz.
- **Conteúdo comprometido** : Se um site for comprometido e contiver conteúdo prejudicial ou irrelevante, o Google poderá tomar medidas manuais para proteger os usuários.


Caso seja necessária uma ação manual, você será notificado pelo Google Search Console.


**Fonte da imagem** :[Google](https://support.google.com/webmasters/thread/7879895/regarding-manual-actions-in-search-console?hl=en)


A notificação fornecerá detalhes sobre o problema. Além de orientações sobre como corrigir o problema e instruções sobre como solicitar uma revisão após as correções serem feitas.


Depois de corrigir o problema, insira-o no Google Search Console e clique em “ **Solicitar revisão** .”


Se você corrigiu o problema corretamente, a equipe do Google o marcará como resolvido e as penalidades poderão ser removidas.


### Problemas de segurança no Google Search Console


Quando são identificados problemas de segurança, eles podem afetar negativamente a visibilidade do seu site nos resultados de busca. Além disso, o Google pode classificar seu site como potencialmente prejudicial aos usuários.


O Relatório de Problemas de Segurança no Google Search Console fornece informações sobre possíveis ameaças ou problemas de segurança detectados em seu site.


Os problemas de segurança mais comuns relatados no Google Search Console incluem:


- **Infecções por malware** : O Google pode detectar malware em seu site, indicando que ele foi comprometido. E existe software malicioso que pode prejudicar os visitantes.
- **Conteúdo hackeado** : Se o seu site foi hackeado, os invasores podem inserir conteúdo ou links indesejados. O Google irá alertá-lo caso seja detectado conteúdo hackeado.
- **Engenharia social** : O Google identifica instâncias em que seu site pode estar envolvido em ataques de engenharia social, como phishing ou práticas enganosas.


### Medidas para solucionar problemas de segurança:


Use o Google Search Console para visualizar e corrigir quaisquer problemas de segurança.


Abra o Google Search Console. Navegue até a seção Ações Manuais de Segurança & no lado esquerdo da tela. Clique em “ **Problemas de segurança** ”.


Se houver algum problema sinalizado, você o verá aqui.


O Google fornecerá informações sobre o tipo de problemas de segurança detectados. Além de recomendações ou ações para resolver os problemas.


Caso seja detectado malware ou conteúdo invadido, limpe e proteja seu site. Remova qualquer código malicioso, atualize as senhas e implemente medidas de segurança adicionais.


Após resolver os problemas de segurança, envie seu site para análise através do Google Search Console. O Google avaliará se os problemas foram resolvidos de forma adequada.


## Prevenindo erros no Google Search Console


Siga estas práticas recomendadas para evitar que erros ocorram no Google Search Console.


- **Realize auditorias regulares no site.** Realize auditorias regulares em seu site para identificar e solucionar problemas técnicos prontamente.
- **Mantenha seu sitemap XML atualizado.** Certifique-se de que seu site possua um sitemap XML que inclua todas as páginas relevantes. Envie o sitemap aos mecanismos de busca por meio do Google Search Console para facilitar a indexação e o rastreamento.
- **Otimize seu arquivo robots.txt.** Utilize um arquivo robots.txt bem estruturado para controlar quais partes do seu site devem ser rastreadas pelos mecanismos de busca. Revise e atualize este arquivo regularmente.
- **Certifique-se de ter um certificado SSL válido.** Certifique-se de que seu site possua um certificado SSL válido, que forneça uma conexão segura (HTTPS). O Google prioriza sites seguros, e isso impacta positivamente o posicionamento nos resultados de busca.
- **Otimizar imagens.** Otimize as imagens para um carregamento mais rápido. Use nomes de arquivo descritivos e inclua texto alternativo para acessibilidade e SEO.
- **Use uma estrutura de URL forte.** Mantenha os URLs simples, descritivos e fáceis de usar. Evite usar parâmetros de URL complexos sempre que possível.


As ferramentas do Semrush ajudam você a ficar por dentro do desempenho técnico de SEO do seu site. Com eles, você pode detectar problemas antes que se agravem.


Use a[Auditoria do Site](https://www.semrush.com/siteaudit//) para saber mais sobre quaisquer problemas atuais com seu site. E como consertá-los.


Além disso, você receberá e-mails automáticos assim que surgir um novo problema em potencial. Assim, você estará sempre no controle do seu desempenho em SEO.
