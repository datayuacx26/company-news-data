---
schema_version: "1.0.0"
document_id: "52f04af31b78226826c14ea96e93e9715ff8e07eb727822b7ecd56d3486a80cf"
company_key: "yc-linkana"
company: "Linkana"
source_id: "yc-linkana-rss-b8a570b33476"
canonical_url: "https://www.linkana.com/blog/engenheiro-de-srm-na-pratica/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.551042+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:d08c08b737766dfcef12cfbe1e0102eb2d301741daaa3c5b5db8aebb8f1e0be8"
---

# Engenheiro de SRM na prática: como funciona a homologação no piloto automático

São 9h de uma segunda. No modelo antigo, o analista de homologação abria a fila com 47 documentos para conferir e passava a manhã lendo alvará linha a linha e perseguindo certidão vencida por e-mail. Hoje, na mesma cadeira, ele abre um painel onde a triagem da madrugada já aconteceu. Doze fornecedores entraram aprovados, com balanço e contrato social lidos e validados. Seis ficaram parados: um teve um sócio que apareceu numa lista de empresas punidas, outro mandou um AVCB fora da validade. O dia dele começa nesses seis.


Essa mudança tem nome. O[engenheiro de SRM](https://www.linkana.com/blog/srm-engineer/) é o profissional que desenha o sistema que decide, em escala, quem entra na base de fornecedores. Quando o operacional da homologação sai das costas da pessoa, é esse o papel que sobra. O nome é novo, mas a virada de função é concreta: ele projeta o critério que roda em toda a base de uma vez e comanda o plano de voo enquanto o software voa a rotina.


Este texto é sobre o como. O que esse profissional configura no dia a dia, e o que a Linkana executa por baixo para que a configuração vire decisão sem ninguém abrir documento na mão.


## O que faz um engenheiro de SRM no dia a dia


O trabalho do engenheiro de SRM começa antes de qualquer fornecedor chegar. Ele desenha a régua da[homologação](https://www.linkana.com/blog/homologacao-de-fornecedores/) .


Na Linkana, essa régua tem um nome: a categoria. Ao montar uma categoria, o engenheiro decide o tipo de fornecedor que ela atende, pessoa jurídica por CNPJ, pessoa física por CPF ou empresa internacional por TIN, e escolhe exatamente o que será exigido de cada um. Quais documentos o fornecedor precisa enviar, e quais consultas e bases públicas a Linkana cruza sozinha por trás. Um fornecedor de limpeza não recebe a mesma régua de um fornecedor que transporta produto químico ou que toca um sistema com dado sensível.


Depois ele calibra o peso de cada exigência. Cada documento da categoria recebe uma pontuação de risco, e essa pontuação define quanto o Score de Risco do fornecedor cai quando aquele documento é reprovado. Uma certidão de débito vencida pesa diferente de um alvará sanitário ausente. É o engenheiro que decide esse peso, e é ele que decide em que momento o monitoramento liga: no início da homologação, depois da aprovação ou depois da reprovação.


Isso é o critério: fica escrito e versionado, e roda igual em milhares de fornecedores sem depender de ninguém lembrar a regra de cabeça.


## O que o software da Linkana executa por baixo


A régua só vira decisão porque existe um[software de SRM](https://www.linkana.com/blog/software-srm/) que faz o trabalho operacional sem cansar. Sem ele, o engenheiro de SRM seria um cargo bonito no papel, porque a pessoa voltaria a conferir documento na mão. São três frentes rodando por baixo.


### Como a Linkana AI lê e valida os documentos


Quando o fornecedor envia um alvará, um balanço patrimonial ou um contrato social, a Linkana AI lê o conteúdo e extrai o que importa: CNPJ, razão social, data de emissão, validade, órgão emissor. Depois valida contra as condições daquele tipo de documento. O documento pertence mesmo a esse fornecedor? Está dentro da validade? Foi emitido pela autoridade certa? A cidade bate com o cadastro? Em segundos, aprova ou reprova, e ainda captura a data de vencimento para controlar a renovação. São agentes especializados por tipo de documento, que reconhecem até as variações regionais: o AVCB de São Paulo, o CLCB do Paraná e o CSCIP do Mato Grosso são o mesmo documento com formatos diferentes, e a IA sabe disso.


### Quais consultas e listas a Linkana cruza


As consultas e bases públicas, os Documentos Inteligentes, cruzam o fornecedor, e os sócios dele, contra dezenas de fontes. Cadastro de empresas inidôneas e punidas, certidão negativa de débitos trabalhistas, regularidade do FGTS, listas de trabalho escravo e trabalho infantil, sanções internacionais, processos cíveis e criminais, pessoas politicamente expostas no quadro societário. Os dados bancários passam por uma validação por Pix, com um depósito de um centavo que confirma que a conta é mesmo do CNPJ cadastrado. Numa análise típica, são dezenas de cruzamentos por fornecedor. Nenhum time faz isso na mão numa base grande sem perder qualidade no caminho.


### Como o monitoramento contínuo funciona


O monitoramento não para na entrada. Aprovado o fornecedor, a Linkana renova as consultas e bases em ciclos próprios, a cada 45, 180 ou 360 dias conforme a fonte, e cria uma nova versão do documento quando detecta mudança. É essa camada que enxerga o[risco que o processo manual nunca vê](https://www.linkana.com/blog/gestao-de-riscos-fornecedores/) : o fornecedor que estava regular e teve um sócio incluído numa lista de sanção depois de já estar operando. Quando uma certidão se aproxima do vencimento, o alerta sai 5 e 3 dias antes. Quando o Score de Risco de um fornecedor desaba para a classificação mais crítica, o aviso chega na hora.


## Piloto automático significa perder o controle?


Vale ser claro sobre isso, porque em compliance enterprise é o que mais pesa: piloto automático não é abrir mão do controle. É o contrário.


No modelo manual, o gestor tem uma sensação de controle que não se sustenta. Ninguém garante que mil fornecedores estão todos em dia o tempo todo conferindo na mão. O controle real aparece quando a régua está escrita na categoria, com peso definido por documento, e quando o sistema avisa no instante em que um sócio entra numa lista ou uma certidão vence. O engenheiro de SRM vê cada evento na linha do tempo do fornecedor, ajusta os pesos quando a política muda e decide os casos de exceção que a máquina separou justamente por exigirem julgamento. A máquina toca a rotina, ele mantém a régua sobre ela.


É por isso que o padrão da área sobe. A pergunta que importa passa a ser uma só: como garantir que a base inteira de fornecedores está em dia o tempo todo, sem ninguém checando na mão? Quem chega nessa pergunta primeiro larga na frente, porque constrói uma operação que aguenta crescer.
