---
schema_version: "1.0.0"
document_id: "e9e879d56245cf52c8505c3c641f2c89ce7c876cce8f05bb5aac93c189f64d2d"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-news-import-7663787d24e6"
canonical_url: "https://pt.semrush.com/blog/o-que-e-google-tag-manager/"
published_at: "2023-09-21T15:12:00+00:00"
first_seen_at: "2026-07-22T13:04:31.954592+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:6d8644e62f00470c3b680b87f73ffd791c826635e22bd94b69f99563a3f012a6"
---

# Google Tag Manager: o Que é e Como Funciona?

## GTM: O Que é o Google Tag Manager?


O[Google Tag Manager (GTM)](https://tagmanager.google.com/#/home) ou Gerenciador de Tags do Google é uma ferramenta gratuita que permite instalar, armazenar e gerenciar tags de marketing sem modificar o código do site.


Tags de marketing são pequenos trechos de código que rastreiam as ações do usuário e coletam dados.


O exemplo mais comum de tag de marketing é a[tag do Google](https://developers.google.com/tag-platform/gtagjs?hl=pt-br) usada para instalar o Google Analytics e outros produtos do Google.


Outros exemplos comuns incluem códigos de evento do Google Analytics, scripts de conversão do Google Ads, código Meta Pixel e tags de remarketing.


Você precisa colocar tags como essas no código do seu site para que o Google Analytics e outras plataformas funcionem e coletem dados.


Mas, com cada adição e alteração de código, você corre o risco de esquecer, perder ou quebrar tags (ou outros elementos do site).


Por isso, o GTM atenua esses problemas, permitindo que você instale, gerencie e implante tags de marketing a partir de seu software, em vez de ter que fazê-lo no código do seu site.


O software insere trechos de código e tags de marketing no código do seu site somente quando necessário.


Assim, você não quebrará acidentalmente o código do seu site ao fazer algum teste.


### Para Que é Usado o Google Tag Manager?


O Tag Manager permite que você rastreie quase qualquer tipo de evento ou comportamento do usuário.


A seguir, veja alguns exemplos de eventos comumente rastreados:


- Cliques em links
- Cliques em botões
- Envios de formulário
- Conversões
- Abandono do carrinho de compras
- Adição de itens ao carrinho
- Remoção de itens do carrinho
- Downloads de arquivos
- Comportamento de rolagem no site
- Visualizações de vídeo
- Desempenho da call-to-action (CTA)
- Cliques no índice (TOC)
- Eventos personalizados


## Como Funciona o Google Tag Manager?


O Google Tag Manager funciona por meio do uso de um único snippet de[código JavaScript](https://www.semrush.com/blog/javascript/) adicionado ao seu site, que vai conter todas as tags que você quiser gerenciar.


Quando um usuário aciona uma tag (por exemplo, ao clicar em um link ou carregar uma página), o GTM a registra e a insere no código do seu site.


Em seguida, o GTM executa a tag.


Em outras palavras, o GTM apenas insere e executa uma tag quando um usuário a aciona. Isso reduz a quantidade total de código em um site e minimiza o risco de erros de implementação.


As tags funcionam por meio de outros componentes do GTM chamados gatilhos e variáveis, que determinam quando os trechos de código devem ser executados.


Abaixo, vamos discutir o que são tags, gatilhos e variáveis e como eles funcionam.


### O Que São Tags?


Tags são trechos de código, marketing e plataformas de suporte usadas para integração com sites e aplicativos.


O[Google Analytics](https://pt.semrush.com/kb/1048-difference-semrush-google-analytics) , por exemplo, usa tags para coletar dados sobre os visitantes do site.


Nesse caso, a tag do Google permite o envio de dados do seu site para uma propriedade conectada do Google Analytics.


Através do GTM, você pode criar, gerenciar e publicar tags sem que você mesmo precise codificá-las.


#### Exemplos de Tags


Veja alguns exemplos de tags comumente usadas. Estas são ferramentas e plataformas frequentemente instaladas com GTM:


- Google Analytics
- Google Ads Conversion Tracking
- [PageImprove](https://pt.semrush.com/pageimprove/)
- Crazy Egg
- Hotjar
- [ImpactHero](https://pt.semrush.com/impacthero/)
- Pinterest Tag
- LinkedIn Insight
- Twitter Universal Web Tag
- Facebook/Meta Pixel
- [SplitSignal](https://pt.semrush.com/splitsignal/)
- Appcues
- HubSpot
- Intercom
- Mixpanel
- Salesforce
- Eventos e rastreamento customizados


### O Que São Gatilhos?


Os gatilhos são as instruções de quando as tags devem ser acionadas.


Visualizações de páginas, envios de formulários e cliques em links são alguns exemplos comuns de gatilhos.


Quando um usuário visualiza uma página ou clica em um link, a tag associada é acionada.


Você precisa atribuir um gatilho a cada tag para que o Tag Manager saiba em que circunstâncias dispará-la.


Plataformas analíticas como o Google Analytics normalmente usam visualizações de página como gatilho. E é importante ter dados analíticos de cada página que um usuário visita.


Como alternativa, você pode querer que uma tag de acompanhamento de conversões seja acionada apenas quando um usuário coloca um item no carrinho, ou talvez quando um processo de checkout for concluído.


Quando o evento especificado em cada tag ocorre, seu gatilho instrui a tag a disparar.


Caso contrário, essa tag não será acionada e o GTM não executará o trecho de código.


Você também pode adicionar filtros aos gatilhos para especificar quando deseja que as tags sejam disparadas.


Por exemplo, você pode querer que seu chat de suporte seja acionado apenas nas páginas do funil de checkout.


Nesse caso, você pode adicionar um filtro que diga à tag para disparar apenas em visualizações de páginas com “/checkout/” no URL.


Você pode atribuir vários gatilhos a uma única tag.


#### Exemplos de Gatilhos


Veja alguns dos gatilhos mais comumente usados:


- Visualizações de página
- Cliques em links
- Cliques em botões
- Envios de formulário
- Profundidade de rolagem no site
- Tempo gasto na página
- Evento personalizado


### O Que São Variáveis?


Variáveis são informações adicionais que o GTM pode precisar para disparar uma tag ou gatilho.


Elas ajudam a definir com precisão o que a tag ou gatilho deve fazer.


Por exemplo, "[Constant](https://www.simoahava.com/analytics/variable-guide-google-tag-manager/#:~:text=The%20Constant%20variable%20returns%20the,type%20in%20the%20Value%20field.&text=The%20obvious%20use%20case%20is,create%20a%20new%20GA%20tag.) " e "Google Analytics Settings" são variáveis comuns.


Ambas são normalmente usadas para definir IDs de conta do Google Analytics (ou seja, "Tracking ID" no Universal Analytics e "Measurement ID" no GA4).


Cada vez que você cria uma tag do Google Analytics, precisa inserir seu ID de rastreamento ou medição.


Isso pode ser meio chato, pois a maioria de nós não memoriza essas identificações.


Por isso, você pode criar uma variável que simplesmente armazene seu ID.


Assim, sempre que você criar uma nova tag que exija essas informações, basta anexar a variável que você criou.


#### Exemplos de Variáveis


A seguir, veja algumas das variáveis mais comuns:


- Constant
- Google Analytics Settings
- Click URL
- Click ID
- Click Class
- Page URL
- Form ID
- Scroll depth threshold


## Benefícios do Google Tag Manager


Embora o Tag Manager exija algum conhecimento técnico, ele economiza muito tempo e recursos dos profissionais de marketing – e é grátis.


Confira alguns benefícios:


- **Redução da dependência de desenvolvedores** : você conseguirá adicionar e gerenciar tags de marketing de forma rápida e simples, sem assistência do desenvolvedor.
- **Teste e depuração** : você poderá visualizar, testar e depurar alterações antes de enviá-las para um site ativo. Esse recurso ajuda a evitar erros e reduz a necessidade de testes em um site ativo ou de teste.
- **Controle de permissão do usuário** : somente pessoas autorizadas podem fazer ou aprovar alterações em sua configuração de marketing.
- **Criação de diferentes versões** : o GTM cria e registra uma nova versão do código sempre que você publica alterações. Isso permite visualizar alterações anteriores e atuais e reverter para versões anteriores, se necessário.
- **Espaços de trabalho** : o GTM permite vários espaços de trabalho para os membros da equipe trabalharem simultaneamente, sem sobrescrever o progresso uns dos outros.
- **Fácil configuração de muitas ferramentas populares** : é possível adicionar facilmente ferramentas e tags de rastreamento ao código do seu site usando snippets de código ou modelos existentes na[Community Template Gallery](https://tagmanager.google.com/gallery/) (por exemplo, para Google Analytics, Pinterest, Google Ads etc.).
- **Armazenamento e gerenciamento de tags** : o GTM consolida tags de marketing e códigos de rastreamento em um só lugar e os insere no código do site conforme necessário. Isso permite que você adicione, remova e atualize ferramentas e códigos de rastreamento com menos erros.


## Google Tag Manager x Google Analytics


Embora muitas vezes confundidos, o Google Tag Manager e o Google Analytics são ferramentas diferentes que você pode usar em conjunto para coletar e gerar relatórios sobre análises da web e comportamento do usuário.


O Google Tag Manager é um sistema de gerenciamento de tags que armazena e gerencia tags de marketing e snippets de código de terceiros – não há análises ou relatórios no Tag Manager.


O Google Analytics, por outro lado, é um software usado para análises, acompanhamento de conversões e relatórios.


Você pode usar o GTM para instalar e implantar o Google Analytics em um site – o[Google, inclusive, recomenda fazê-lo](https://developers.google.com/tag-platform/devguides?hl=pt-br#:~:text=To%20tag%20a%20website%20with%20a%20supported%20Google%20product%2C%20we%20recommend%20using%20either%20the%20gtag.js%20library%20or%20Tag%20Manager.%20To%20learn%20more%20about%20choosing%20the%20right%20option%2C%20read%20%22Google%20Tag%20Manager%2C%20Google%20tag%2C%20or%20Firebase%3F%22) .


### É Preciso Alterar Alguma Coisa no Google Analytics 4?


Sim. Se estiver usando o GTM para instalar o Google Analytics 4 (GA4), você precisará instalar a tag "GA4 Configuration".


Fora isso, usar o Google Tag Manager com GA4 é muito semelhante a usá-lo com o Universal Analytics (UA).


Uma das principais diferenças é que o Google agora fornece uma tag separada para eventos GA4.


Anteriormente, você implementaria o UA e rastrearia eventos usando a mesma tag.


Outra diferença é que o GA4 pode rastrear e coletar automaticamente muito mais eventos do que o UA, o que significa menos necessidade de rastreamento manual de eventos no GTM. No entanto, você ainda pode usar o GTM para rastrear algo que o GA4 não possibilite.


O Tag Manager e o GA4 funcionam perfeitamente juntos.


**Dica** : leia o guia do Google sobre a[transição do UA para o GA4](https://support.google.com/analytics/answer/10759417?hl=en&ref_topic=10737980) para obter informações adicionais.


## Como Configurar o Google Tag Manager


Para configurar o Google Tag Manager, faça login na sua conta do Google e vá para o[Tag Manager](https://tagmanager.google.com/) .


Clique em " **Create Account** " para criar uma conta para sua empresa ou organização.


Digite um nome para a conta e selecione seu país. O Google recomenda que cada empresa crie apenas uma conta, quer você tenha um site ou vários. O nome da sua empresa pode servir como nome da sua conta.


Em seguida, insira um "Container Setup".


Esse elemento é o código que você adicionará ao seu site para fazer o Google Tag Manager funcionar. Normalmente, você usará um container por site. Portanto, o nome do seu container pode ser o nome ou URL do seu site.


Em seguida, selecione sua "Target platform". Para sites, escolha " **Web** ".


Clique em " **Create** " para criar sua conta do Tag Manager e seu primeiro container.


A seguir, você verá dois pop-ups.


O primeiro são os termos de serviço do Google. Concorde com os termos marcando a caixa na parte inferior da página e clicando em " **Yes** " para aceitar os termos de serviço no canto superior direito da tela.


O segundo pop-up exibe os trechos de código que você precisa adicionar ao seu site para instalar o GTM.


**Observação** : se você não se sentir confortável em modificar o código do seu site, peça ajuda a um desenvolvedor.


O primeiro trecho de código usa JavaScript para extrair informações. Coloque esse snippet o mais alto possível na seção <head> das páginas do seu site.


O segundo trecho é um iframe HTML. Coloque esse código imediatamente após a tag de abertura <body> em suas páginas da web.


Assim:


(O elemento HTML iframe garante que o GTM ainda funcione mesmo se os usuários desabilitarem o JavaScript em seus navegadores.)


Depois de configurar sua conta e instalar corretamente os dois snippets de código, você estará pronto para começar a usar o Google Tag Manager.


**Dica** : use a[extensão Tag Assistant Legacy](https://chrome.google.com/webstore/detail/tag-assistant-legacy-by-g/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=en) para garantir que a implementação do Tag Manager esteja correta.


## Como Usar o Google Tag Manager


Para mostrar como usar o Google Tag Manager, ensinaremos você a conectar uma propriedade do Google Analytics 4 ao seu site.


**Dica** : para evitar que suas análises registrem resultados duplos, certifique-se de ter[instalado o Google Tag Manager](https://support.google.com/tagmanager/answer/6103696?hl=en) e removido o código original do Google Analytics do seu site antes de continuar.


Para começar, faça login em sua conta do[Google Tag Manager](https://tagmanager.google.com/) .


Clique em " **New Tag** " ou em " **Add a new tag** " para criar uma nova tag.


Adicione um nome para sua tag. Sugerimos algo como "Configuração GA4".


Em seguida, clique em " **Tag Configuration** ".


Após fazer isso, selecione " **Google Analytics: GA4 Configuration** " na lista.


Introduza o[ID de mensuração](https://support.google.com/analytics/answer/9539598#find-G-ID) da sua propriedade do GA4. Como alternativa, você pode criar uma variável para armazenar seu ID de mensuração para uso futuro.


Depois, clique na caixa " **Triggering** ".


Selecione " **All Pages** " na lista que aparece.


Em seguida, clique em " **Save** ".


Volte para a página de visão geral e clique em " **Preview** " para testar as alterações.


Insira a URL do seu site na próxima tela e clique em " **Connect** " para abrir seu site em uma nova janela e começar a testar e depurar.


Assim que estiver conectado, seu site deverá abrir em um novo pop-up com o Tag Assistant no canto inferior direito da sua janela.


Mantenha essa janela aberta e retorne à página de visualização. A página de visualização deve dizer "Connected!"


Depois de clicar em " **Continue** ", comece a depurar suas tags na janela de visualização.


Você deverá ver sua tag de configuração GA4 recém-criada sendo disparada em cada página enquanto você navega em seu site.


Caso isso aconteça, significa que a implementação da sua tag foi bem-sucedida.


Quando terminar a depuração, feche as janelas recém-abertas e volte para a visão geral da página. Clique em " **Submit** " para enviar as alterações.


Certifique-se de criar um nome e uma descrição para essa versão das alterações.


Por último, clique em " **Publish** " para enviar as alterações para seu site e ativar o GA4.


Com isso, você[publicará uma nova versão do container](https://support.google.com/tagmanager/answer/6107163?hl=en) em seu site.


Agora, deve ser possível ver os registros no relatório "Realtime" da sua propriedade GA4 enquanto você e outros usuários navegam no site.


**Dica** : recomendamos sempre usar a função de visualização para testar e depurar suas tags antes de publicá-las.


## Melhores Extensões GTM


As extensões do navegador ajudam você a aproveitar melhor o Tag Manager.


As extensões a seguir fornecem informações adicionais sobre as tags que estão sendo disparadas (ou não) e quaisquer problemas que surjam com suas implementações.


Elas também oferecem detalhes importantes sobre suas variáveis, gatilhos e trechos de código.


As extensões a seguir são algumas das melhores para facilitar o uso do GTM:


- [Tag Assistant](https://chrome.google.com/webstore/detail/tag-assistant-by-google/kejbdjndbnbjgmefkgdddjlbokphdefk)
- [GTM/GA Debugger](https://chrome.google.com/webstore/detail/gtmga-debug/ilnpmccnfdjdjjikgkefkcegefikecdc)
- [Dataslayer](https://chrome.google.com/webstore/detail/dataslayer/ikbablmmjldhamhcldjjigniffkkjgpo)
- [DataLayer Inspector+](https://chrome.google.com/webstore/detail/adswerve-datalayer-inspec/kmcbdogdandhihllalknlcjfpdjcleom)
- [Da Vinci Tools](https://chrome.google.com/webstore/detail/da-vinci-tools/pekljbkpgnpphbkgjbfgiiclemodfpen?hl=en)


## Simplifique seu Marketing


O Google Tag Manager é uma ferramenta versátil que pode simplificar o processo de instalação e gerenciamento de ferramentas de marketing e rastreamento de eventos importantes.


Recomendamos que você veja algum[tutorial do Google Tag Manager](https://analytics.google.com/analytics/academy/course/5) antes de migrar totalmente para a plataforma.


Depois de se familiarizar com ele, o Tag Manager será uma adição maravilhosa a qualquer kit de ferramentas de marketing.


Combine o GTM com ferramentas poderosas como o Google Analytics, o Google Search Console e a[Auditoria do site](https://pt.semrush.com/siteaudit/) da Semrush e você terá tudo o que precisa para executar uma estratégia de marketing simplificada.


Compartilhar


[Zach Paruch](https://pt.semrush.com/blog/user/203642673/)


Zach Paruch is a data-driven SEO strategist with 10+ years of experience driving organic growth through smart, scalable search strategies. His expertise includes on-page and technical SEO, AI search optimization, and content strategy—with a special focus on ideating and implementing AI-driven processes. By leveraging in-depth search intent analysis, refined information architecture, and user-centered design, Zach consistently delivers high-impact content that drives business outcomes.
