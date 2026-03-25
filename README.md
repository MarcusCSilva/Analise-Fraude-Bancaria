### Análise de Dados: Detecção de Padrões em Fraudes de Cartão de Crédito


**Sobre o Projeto**

O crescimento das transações digitais trouxe consigo um aumento expressivo em tentativas de fraude financeira. Este projeto é um pipeline analítico ponta a ponta (End-to-End) focado na detecção de padrões de comportamento criminal, utilizando um dataset massivo de transações de cartão de crédito.
O objetivo principal foi construir uma infraestrutura de dados na nuvem para processar a base original (+500 mil registros), aplicar regras de negócio no banco de dados e construir um painel visual para identificar as faixas de valores mais visadas, os tipos de comércio preferidos pelos criminosos e o volume de perdas financeiras.

**Ferramentas Utilizadas**

* Linguagens: Python e SQL (Dialeto Presto)
* Bibliotecas: Pandas
* Infraestrutura Cloud: AWS S3 (Data Lake) e Amazon Athena (Query Engine)
* Visualização e BI: Power BI e Power Query

**Tratamento, Limpeza de Dados e Engenharia (Data Pipeline)**

A base de dados passou por um rigoroso processo de estruturação, migrando de um ambiente de script (Python) para uma arquitetura escalável em Nuvem (AWS). As principais etapas foram:

* Adequação de Delimitadores (Blindagem de CSV): Identificação de que o uso tradicional de vírgulas (.csv) causaria o deslocamento de colunas devido aos nomes dos comércios. O delimitador foi alterado para Pipe (|) via Pandas antes da ingestão na nuvem, garantindo a integridade da tabela.
* Modelagem no AWS Athena (External Tables): Criação de uma tabela conectada diretamente ao bucket S3, permitindo consultas via SQL sem a necessidade de importar os dados para um banco tradicional.
* Engenharia de Regras de Negócio (Views e Pushdown): Criação de uma View (Tabela Virtual) utilizando a instrução CASE WHEN para categorizar o valor absoluto das transações (amt) em faixas de valores específicas (ex: "0 a 50", "500-1000"). Essa transformação foi feita no banco de dados para aliviar o processamento na camada do Power BI.
* Tratamento de Ordenação (Power Query): Criação de uma Coluna Condicional no Power BI para associar índices numéricos às categorias de texto, resolvendo o problema de ordenação alfabética e garantindo a correta exibição progressiva dos filtros.

**Principais Questionamentos de Negócio**

Durante a análise, busquei responder às seguintes perguntas para auxiliar na tomada de decisão da equipe antifraude:

* Qual é o volume financeiro total movimentado por fraudes em comparação ao mercado legítimo?
* Quais faixas de valor financeiro são as mais escolhidas pelos fraudadores?
* Quais são as categorias de lojas e comércios mais afetadas por esses ataques criminosos?
* Há distinção no volume de ataques entre gêneros de clientes e localizações geográficas?

**Resultados e Insights**

**1. A Zona de Risco Intermediária ($500 a $1000)**
A análise revelou que os criminosos evitam realizar transações com valores exorbitantes logo de início para não acionar os algoritmos automáticos de bloqueio dos bancos. A faixa de $500 a $1000 demonstrou ser a "zona de conforto" preferida para a extração do dinheiro roubado, apresentando o maior número de ocorrências confirmadas.

**2. Identificação de "Card Testing" ($0 a $50)**
Um volume expressivo de fraudes foi detectado em transações extremamente baixas, na faixa de $0 a $50. Este comportamento indica a prática de Card Testing, onde o fraudador realiza pequenas compras (como em supermercados ou lojas de conveniência) apenas para validar se o cartão clonado está ativo e possui limite, antes de realizar o golpe maior na Zona de Risco.

**3. O Alvo Favorito por Categoria**
Ao cruzar a faixa de valores mais visada com os tipos de comércio, constatou-se que a categoria "Shopping Net" (Compras Online) é o vetor principal de escoamento. O anonimato das compras virtuais facilita o uso do cartão clonado em detrimento das compras presenciais que exigem aproximação física.

