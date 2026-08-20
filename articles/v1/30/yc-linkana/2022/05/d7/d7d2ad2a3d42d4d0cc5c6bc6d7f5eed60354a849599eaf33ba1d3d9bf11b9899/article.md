---
schema_version: "1.0.0"
document_id: "d7d2ad2a3d42d4d0cc5c6bc6d7f5eed60354a849599eaf33ba1d3d9bf11b9899"
company_key: "yc-linkana"
company: "Linkana"
source_id: "yc-linkana-rss-b8a570b33476"
canonical_url: "https://www.linkana.com/blog/cnpj-invalido/"
published_at: "2022-05-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.551042+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:e08a1d7cb7d27c823d9bd68ad1c18949732d19933cb3e13fb642cb32400a0651"
---

# CNPJ inválido: por que acontece e como resolver o problema?

O Cadastro Nacional da Pessoa Jurídica ([CNPJ](https://cnpj.linkana.com/) ) é a primeira identificação que uma empresa ganha quando é formalizada na Receita Federal. Por isso, quando alguma operação financeira retorna com “ **CNPJ inválido** ”, é importante saber o motivo dessa ocorrência.


Isso porque o CNPJ não só comprova que a empresa é formalizada, mas também indica se a sua **situação cadastral** está ativa ou não. Dessa forma, você consegue pesquisar sobre um fornecedor ou parceiro de negócio, por exemplo, antes de fechar um contrato com eles.


Quer entender melhor o porque o CNPJ dá inválido? Continue a leitura deste artigo e saiba quando pode acontecer.


## O que significa CNPJ inválido?


**CNPJ inválido significa que o número de cadastro da empresa apresenta algum erro numérico, impossibilitando a sua identificação no sistema da Receita Federal** .


Por exemplo, quando uma nota fiscal precisa ser emitida e o CNPJ que foi digitado não é localizado, o sistema utilizado para emissão (ERP ou site da prefeitura) retorna com um aviso de CNPJ inválido.


Esse erro pode acontecer tanto em relação ao emitente quanto ao destinatário da nota fiscal. Por isso, é importante ter o banco de dados de todos os fornecedores e parceiros sempre atualizado.


## Por que o CNPJ dá inválido?


Existem três motivos principais para que o CNPJ fique inválido. São eles:


- **Tamanho do CNPJ** : o número de cadastro possui 14 caracteres numéricos. Então, se o sistema suprime um número, geralmente o zero, o CNPJ dá inválido;


- **Posicionamento errado de algum número** : seja por um erro digitação ou por uma inversão do sistema de emissão de notas que pode trocar a posição de um caractere;


- **Erro no dígito verificador** : são os dois últimos dígitos que indicam a autenticidade do CNPJ e que se digitado errado gera um erro.


## 4 exemplos de CNPJ inválido e como resolvê-los


Para entender melhor as situações que podem indicar CNPJ inválido, trouxemos alguns exemplos e os códigos de rejeições que identificam a inconsistência no cadastro. Confira:


### Exemplo 1: rejeição 207


Suponhamos que o CNPJ de uma empresa comece zero, por exemplo: 02.345.678/001-90. Ao digitar o número, o sistema de emissão de nota fiscal pode **suprimir o zero inicia** l e quando é feita a busca no banco de dados da Secretaria de Fazenda, o CNPJ chega como “2.345.678/001-90”.


Isso gera um erro identificado como **rejeição 207** , que significa CNPJ do emitente inválido.


Caso o sistema inverta a posição do zero para “20” ou suprima o “0” do dígito verificador, o mesmo código vai aparecer indicando o CNPJ inválido.


Nesse caso, o primeiro passo é verificar se o CNPJ foi digitado corretamente ou se o sistema fez alguma dessas alterações no número e, então, contatar o suporte do serviço para reportar o erro.


### Exemplo 2: rejeição 208


Outro caso em que o CNPJ dá inválido é quando uma das intervenções acima acontece em relação ao **CNPJ do destinatário** .


Para orientar quem está emitindo a nota, o sistema atribui códigos de erros diferentes para deixar claro onde está a inconsistência da informação.


Dessa forma, a **rejeição 208** indica que o CNPJ do destinatário inválido, ou seja, o cliente, fornecedor ou parceiros informou o código errado ou houve um erro de digitação no cadastro.


Para corrigir o erro e conseguir finalizar a nota fiscal, é preciso ter o número do CNPJ correto do destinatário.


### Exemplo 3: rejeição 422


Outro erro relacionado ao CNPJ inválido é o identificado com o código **rejeição 422** . Ele acontece quando uma NF3-e precisa ser emitida, mas o número do destinatário está com algum erro de preenchimento, na posição dos zeros ou no dígito verificador.


A correção é feita da mesma forma das situações anteriores, basta verificar o número do destinatário para confirmar porque o CNPJ dá inválido. Feito isso, volte ao sistema de emissão de nota para finalizar o documento.


### Exemplo 4: rejeição 258


O erro identificado como **rejeição 258** (CNPJ da consulta inválido) acontece quando um CNPJ inválido é informado em uma busca no WebService de Consulta de Cadastro.


Esse sistema de consulta, geralmente, é integrado a softwares de automação para realizar consultas públicas em bancos de dados oficiais.


Os critérios que invalidam o CNPJ são os mesmos citados acima, então, para conseguir fazer a pesquisa é preciso identificar e corrigir o erro.


**DICA** : você consegue utilizar o serviço de[consulta de CNPJ da Linkana](https://cnpj.linkana.com/?utm_source=blog_post) para obter informações relacionadas ao CNPJ de uma empresa, dados relacionados ao quadro societário e filiais relacionadas.


## Qual a importância de um CNPJ válido?


Como destacamos na abertura do artigo, o CNPJ é o primeiro passo para a **abertura e formalização** de uma empresa. Com o cadastro ativo, uma organização pode ter relações comerciais no mercado, passando mais credibilidade e confiança.


Além disso, é por meio do número de identificação que uma empresa que precisa contratar um fornecedor confiável pode pesquisar sobre as opções que encontrar.


Hoje, existem sistemas que fazem isso de forma automatizada, buscando nos sistemas da Receita Federal diversas informações relevantes sobre os potenciais parceiros de negócio.


Essas ferramentas ajudam a evitar erros como o CNPJ inválido e outros que também são relacionados às informações cadastrais e podem acontecer na hora de emitir uma nota fiscal.


Com documentos emitidos corretamente, as[auditorias fiscais](https://www.linkana.com/blog/auditoria-fiscal/) que são realizadas acontecem sem nenhuma preocupação e garantem que sua empresa opere 100% dentro das leis existentes e mantenha parceiros que têm a mesma preocupação.


## Facilite as consultas de CNPJ na sua empresa


Quando o CNPJ inválido acontece no início do processo de qualificação e homologação de fornecedores é muito mais fácil contornar o problema ou mesmo descartar um parceiro que não está enquadrado corretamente.


Para fazer essa avaliação de Compliance e governança de forma mais ágil e eficiente, recomendamos a tecnologia da[Linkana](https://www.linkana.com/) para automatizar as consultas públicas sobre qualquer empresa parceira.


Assim, você pode concentrar os esforços do seu[setor de compras](https://blog.linkana.com/como-estruturar-um-departamento-de-compras/) nas avaliações e decisões estratégicas que realmente importam. O nosso sistema possui:


- Robôs especializados em consultas públicas e emissão de certidões de CNPJs;
- Aplicação de machine learning nas análises de informações privadas enviadas pelo fornecedores;
- Monitoramento e atualização permanente de informações de fornecedores homologados;
- Armazenamento e organização de documentos e histórico de informações para auditoria.


Simplifique a[gestão de fornecedores](https://www.linkana.com/blog/gestao-de-fornecedores/) com a Linkana, implementando um processo automatizado de ponta a ponta.
