# Tipos de dados (Estruturados, Semi Estruturados, Não Estruturados)
## ✅ Tipos de Dados: Conceitos Fundamentais
A categorização dos dados em três tipos principais é essencial para entender como armazená-los, processá-los e analisá-los:
### 1. *Dados Estruturados*
- **Definição:** Dados organizados em esquemas fixos (linhas e colunas), normalmente encontrados em bancos de dados relacionais.    
- **Características:**    
    - Estrutura rígida e conhecida.        
    - Facilmente consultável via SQL.        
    - Colunas com tipos de dados definidos.        
- **Exemplos:**    
    - Bancos de dados como **PostgreSQL, MySQL, Oracle, Redshift**.        
    - Arquivos **CSV** (desde que bem formados).        
    - Planilhas do **Excel** com estrutura coerente.     
### 2. *Dados Não Estruturados*
- **Definição:** Dados sem esquema predefinido, difíceis de consultar diretamente.    
- **Características:**    
    - Necessitam pré-processamento e extração de metadados.        
    - Não há formato tabular ou de colunas fixas.        
- **Exemplos:**    
    - Arquivos de **áudio, vídeo, imagem**.        
    - **Textos livres**, como artigos, livros, postagens.        
    - **E-mails** (especialmente corpo do e-mail).        
    - **PDFs escaneados**, gravações de chamadas, etc.        
<div style="page-break-after: always;"></div>
### 3. *Dados Semiestruturados*

- **Definição:** Dados com alguma organização interna (tags, delimitadores, etc.), mas não em formato relacional tradicional.    
- **Características:**    
    - Possuem estrutura parcial, normalmente hierárquica.        
    - Permitem extração de schema de forma dinâmica (schema-on-read).        
    - São comuns em pipelines de dados modernos.        
- **Exemplos:**    
    - Arquivos **JSON, XML**.        
    - **Logs de servidores** (Apache, CloudWatch, etc.).        
    - **Cabeçalhos de e-mails**.        
    - Arquivos **YAML**, **Avro**, **Parquet** (quando usados sem schema explícito no catálogo).        

---
## 🔍 Complementos e Melhorias Sugeridas
### *Tipos de dados modernos utilizados em Engenharia de Dados:*
- **Avro, Parquet, ORC**:    
    - Embora sejam formatos binários, são considerados **semiestruturados** quando schema é lido dinamicamente.        
    - Suportam compressão e colunas com diferentes tipos.        
    - Muito utilizados em data lakes (S3 + Glue + Athena).        
### *Schema-on-Read vs. Schema-on-Write*:
- **Schema-on-Write:** Obrigatório para dados estruturados (ex: banco relacional).    
- **Schema-on-Read:** Comum para dados semiestruturados em data lakes (ex: arquivos JSON lidos pelo Athena).    
### *Ferramentas para manipulação de tipos de dados:*
- **AWS Glue DataBrew, Glue Schema Registry, Amazon Macie** para tipos sensíveis e classificação.    
- **OpenSearch** para dados semiestruturados com consultas full-text.    
- **Tesseract OCR** para extrair texto de PDFs/imagens (transformando não estruturado em estruturado).    
<div style="page-break-after: always;"></div>
### *Casos de uso reais:*
- **Pipeline de logs (semiestruturado)**: leitura de arquivos no S3 → transformação com PySpark → ingestão no Redshift.    
- **Treinamento de IA (não estruturado)**: extração de áudio/imagem para entrada em modelos de ML.
# Características dos dados (os V's)
### 📌 Resumo dos 3 V's do Big Data
#### 1. *Volume*
Refere-se à **quantidade total de dados** gerados e armazenados. Pode variar de gigabytes até petabytes ou mais.
- **Impacto:** influencia o modelo de armazenamento, processamento e transporte dos dados.    
- **Exemplos:**    
    - Mídias sociais gerando terabytes diários com postagens, vídeos e imagens.        
    - Varejistas com anos de dados de transações, totalizando petabytes.        
- **Decisões comuns:**    
    - Uso de soluções como **AWS Snowball ou Snowmobile** para transporte físico de grandes volumes.        
    - Adoção de arquiteturas distribuídas como **Amazon EMR, S3, Redshift** para escalabilidade.        
#### 2. *Velocidade*
É a **rapidez com que os dados são gerados, coletados, processados e analisados**.
- **Impacto:** define se o processamento será em **batch**, **quase em tempo real** ou **tempo real**.    
- **Exemplos:**    
    - Dados de sensores (IoT), atualizados a cada milissegundo.        
    - Sistemas de **trading de alta frequência**, onde cada milissegundo conta.        
- **Decisões comuns:**    
    - Escolha entre ferramentas como **Kinesis Data Streams (tempo real)** vs **Kinesis Firehose (quase tempo real)**.        
    - Considerações sobre **latência**, **ordenação de eventos**, e **consistência**.        
<div style="page-break-after: always;"></div>
#### 3. *Variedade*
Diz respeito aos **diferentes tipos e formatos de dados** com os quais você trabalha.
- **Tipos de dados:**    
    - **Estruturados**: Tabelas, bases relacionais (SQL).        
    - **Semiestruturados**: JSON, XML.        
    - **Não estruturados**: PDFs, vídeos, áudios, imagens, e-mails.        
- **Exemplos:**    
    - Empresa que analisa dados estruturados (bancos), semiestruturados (JSONs) e não estruturados (e-mails).        
    - Sistemas de saúde que lidam com dispositivos médicos, prontuários e formulários.        
- **Desafio:** Unificar diferentes fontes e formatos em um repositório ou camada de análise comum (ex: **Athena**, **Glue**, **Lake Formation**).    
### 📌 Outros V's frequentemente citados
Embora o exame AWS enfatize os **3 V's principais**, outros V’s também são discutidos na literatura de Big Data:
- **Veracidade (4º V):** confiança e qualidade dos dados (dado ruidoso, incompleto, inconsistente).    
- **Valor (5º V):** a utilidade que os dados proporcionam para tomada de decisão.    
- **Variabilidade:** flutuação nos padrões de dados ao longo do tempo.    
- **Visualização:** a capacidade de transformar dados complexos em insights visuais acessíveis.    
### ✅ Dica para a prova AWS Data Engineer
Concentre-se em:
- **Volume →** Decisões de armazenamento e movimentação.    
- **Velocidade →** Escolha entre soluções de streaming vs batch.    
- **Variedade →** Ferramentas e estratégias para dados heterogêneos.
<div style="page-break-after: always;"></div>
# Datawarehouses vs Data Lakes vs Lakehouses
### 📊 *Data Warehouse (DWH)*
- **Conceito**: Repositório centralizado e estruturado, otimizado para análises e consultas complexas.    
- **Formato de Dados**: Apenas dados **estruturados**.    
- **Processamento**: Utiliza o modelo **ETL** (Extração, Transformação e Carga).    
- **Schema**: **Schema on write** – o esquema precisa estar definido antes da carga.    
- **Desempenho**: Alta performance em consultas analíticas.    
- **Casos de uso**: Business Intelligence, relatórios corporativos, análise de vendas, etc.    
- **Exemplo na AWS**: [Amazon Redshift](https://aws.amazon.com/redshift/)    

**Pontos de atenção para o engenheiro de dados**:
- Planejar a modelagem de dados (ex: estrela, floco de neve).    
- Gerenciar mudanças no schema com cuidado (pode ser custoso).    
- Otimizar consultas e índices.    
- Garantir integridade e qualidade dos dados durante o ETL.    
### 🌊 *Data Lake (DL)*
- **Conceito**: Repositório de dados brutos no formato original.    
- **Formato de Dados**: Dados **estruturados, semiestruturados e não estruturados**.    
- **Processamento**: Utiliza o modelo **ELT** (Extração, Carga e Transformação posterior).    
- **Schema**: **Schema on read** – o esquema é inferido na leitura.    
- **Flexibilidade**: Muito mais ágil para ingestão de dados de diversas fontes.    
- **Casos de uso**: Ciência de dados, machine learning, descoberta de dados.    
- **Exemplo na AWS**: Amazon S3 + AWS Glue + Amazon Athena    

**Pontos de atenção para o engenheiro de dados**:
- Garantir organização via **partições**, **prefixos** e **nomenclatura padronizada**.    
- Tratar problemas de qualidade e padronização de dados posteriormente.    
- Criar e manter o **catálogo de dados (ex: AWS Glue)**.    
- Cuidar do versionamento e controle de permissões (ex: via Lake Formation).    
<div style="page-break-after: always;"></div>
### 🏡 *Data Lakehouse (DLH)*
- **Conceito**: Arquitetura híbrida que combina a flexibilidade do DL com a performance do DWH.    
- **Formato de Dados**: Suporta tanto dados estruturados quanto não estruturados.    
- **Schema**: Suporte a **schema on write e on read**.    
- **Processamento**: Transações ACID, controle de versionamento e esquema evolutivo.    
- **Casos de uso**: BI + ML sobre o mesmo repositório, unified analytics.    
- **Exemplo na AWS**: AWS Lake Formation + S3 + Redshift Spectrum  
    Outras tecnologias: Delta Lake (Databricks), Apache Hudi, Apache Iceberg.    

**Pontos de atenção para o engenheiro de dados**:
- Definir formato de armazenamento (ex: Parquet, ORC).    
- Usar formatos transacionais (ex: Hudi, Delta, Iceberg) para time-travel, updates, deletes.    
- Cuidar da performance com particionamento e ordenação de dados.    
- Monitorar metadados, compactação e versionamento.    
### 🧠 *Considerações estratégicas para o engenheiro de dados*
- **Escolha entre DWH, DL ou DLH** depende do tipo de dado, volume, variedade, velocidade e finalidade:    
    - DWH: BI e relatórios sobre dados bem definidos.        
    - DL: Armazenamento flexível e barato com exploração posterior.        
    - DLH: Plataforma unificada para análises avançadas e operacionais.        
- **Custo vs Performance**: DWH tende a ser mais caro, mas muito performático. DL é mais barato, mas requer esforço na leitura. DLH busca o equilíbrio.    
- **Governança de dados**: Implementar controle de acesso, versionamento e catalogação é essencial em DL e DLH.    
- **Qualidade e observabilidade**: Ter pipelines de validação, alertas e monitoramento.    
- **Evolução do schema**: Principal desafio nos DLs; usar formatos como Iceberg ou Hudi ajuda bastante.
<div style="page-break-after: always;"></div>
# O que é um Data Mesh
## 📌 O que é Data Mesh?
O **Data Mesh** (ou "malha de dados") é uma **abordagem descentralizada** à arquitetura e governança de dados, que propõe tratar os dados como **produtos** e distribui a responsabilidade pela governança e disponibilização de dados para os **times de domínio** (_times de negócio_) que melhor conhecem esses dados (ex: vendas, financeiro, logística).

Diferente das arquiteturas tradicionais centralizadas, o Data Mesh **não foca em tecnologia**, mas sim em **organização e cultura**.

> Como diz o instrutor no material:  
> “Trata-se mais de governança e organização do que de tecnologias reais.”
## 🧱 Pilares (Conceitos) do Data Mesh

|Pilar|Explicação|
|---|---|
|**Domínio orientado a dados**|Cada equipe é responsável pelos dados do seu próprio domínio.|
|**Dados como produto**|Os dados devem ser mantidos como produtos utilizáveis por outros times.|
|**Plataforma de dados self-service**|Ferramentas e infraestrutura para permitir que os times publiquem e consumam dados sem dependências externas.|
|**Governança federada**|Cada domínio aplica regras locais, mas segue padrões centrais de segurança e interoperabilidade.|
## 🛠️ Importância do Data Mesh na Engenharia de Dados

### ✅ *Benefícios*
- **Escalabilidade**: Evita que um único time de dados central vire gargalo.    
- **Autonomia**: Os domínios podem evoluir e publicar dados sem dependências.    
- **Contextualização**: Quem entende os dados é quem os disponibiliza.    
- **Governança e segurança distribuídas**: Mantém controle sem centralizar tudo.
### ⚠️ *Desafios*
- Requer **maturidade organizacional e cultural**.    
- Precisa de **plataforma robusta** (ex: AWS Lake Formation, Glue, S3).    
- A governança precisa ser **federada com padrões claros**.    
<div style="page-break-after: always;"></div>
## 🔁 Comparação: Data Mesh vs Star Schema vs Snowflake Schema

| Característica                      | **Data Mesh**                                                           | **Star Schema**                                       | **Snowflake Schema**                                                 |
| ----------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------------------- |
| **Modelo lógico**                   | Descentralizado e orientado a domínios                                  | Centralizado, desnormalizado                          | Centralizado, normalizado                                            |
| **Modelo conceitual**               | Descentralizado e orientado a domínio                                   | Centralizado em um Data Warehouse                     | Centralizado com normalização de dimensões                           |
| **Desenho de dados**                | Dados como produto, expostos por domínios com contratos de dados        | Tabela fato central com dimensões em volta            | Similar ao Star, mas dimensões são normalizadas em múltiplas tabelas |
| **Foco**                            | Organização, governança e autonomia dos domínios                        | Performance de leitura e simplicidade de modelagem    | Redução de redundância e integridade dos dados                       |
| **Responsabilidade por dados**      | Times de negócio (domínios)                                             | Time central de engenharia de dados                   | Time central de engenharia de dados                                  |
| **Escalabilidade organizacional**   | Alta – múltiplos domínios publicando dados                              | Média – depende do time de dados central              | Média – semelhante ao Star                                           |
| **Governança**                      | Federada com padrões centrais                                           | Centralizada                                          | Centralizada                                                         |
| **Infraestrutura típica**           | Data Lakes + Glue + Lake Formation + APIs                               | Data Warehouse (Ex: Redshift, BigQuery, Snowflake DW) | Data Warehouse com estrutura mais normalizada                        |
| **Tecnologia imposta**              | Não impõe tecnologia, é um paradigma organizacional                     | Implica uso de DWHs                                   | Implica uso de DWHs com modelagem mais rigorosa                      |
| **Facilidade de manutenção**        | Exige padrões e governança forte                                        | Simples de manter                                     | Mais complexa, mas com menor redundância                             |
| **Facilidade de uso por analistas** | Alta (desde que bem documentado e com catálogo)                         | Alta (modelo intuitivo)                               | Média (exige conhecimento da estrutura)                              |
| **Facilidade de consumo**           | Alta para usuários com conhecimento de domínio; exige padrão de entrega | Alta – modelo fácil de entender                       | Média – estrutura mais complexa, mas mais robusta                    |
Fonte: ChatGPT
<div style="page-break-after: always;"></div>

## 🧬 Semelhanças e Convivência
- **Complementares**: É possível ter um **modelo Star ou Snowflake dentro de um domínio do Data Mesh**.    
- **Todos exigem catálogos de dados e controle de acesso**, especialmente para uso em larga escala.    
- A **diferença central está na arquitetura organizacional**: enquanto Star e Snowflake focam na modelagem técnica, o Data Mesh redefine quem possui os dados e como são compartilhados.    
## 🎯 Conclusão
O **Data Mesh** é uma **resposta organizacional à necessidade de escalar dados em grandes empresas**, permitindo que cada domínio seja responsável pelos dados que conhece e controla.

> Como reforça o material:  
> “É uma espécie de mundo descentralizado em que os dados pertencem às equipes que mais sabem sobre eles.”

Comparado aos modelos **Star Schema** e **Snowflake**, o Data Mesh **não busca substituir** esses modelos técnicos, mas sim **reposicionar a responsabilidade** sobre os dados, alinhando arquitetura de dados com a estrutura organizacional.
# Gerenciando e Orquestrando Pipelines
## 🔄 O que é um Pipeline de Dados?
Um **pipeline de dados** é um conjunto de etapas organizadas para **movimentar, transformar e disponibilizar dados** de forma contínua ou em lote, desde a origem até o destino (como um Data Lake, Data Warehouse ou sistema de consumo analítico). Essas etapas geralmente incluem:
1. **Extração** de dados de fontes diversas;    
2. **Transformação** para adequação, limpeza, enriquecimento;    
3. **Carga** em destinos otimizados para análise ou persistência.    
<div style="page-break-after: always;"></div>
## 🔧 ETL e ELT: Conceitos Fundamentais
O conteúdo do arquivo fornecido detalha os dois principais padrões:
### 📦 ETL (Extract, Transform, Load)
- **Extração**: captura dos dados brutos de fontes diversas — bancos de dados, APIs, arquivos, etc.;    
- **Transformação**: limpeza, enriquecimento, deduplicação e padronização dos dados antes de serem carregados;    
- **Carga**: os dados já transformados são armazenados no destino, geralmente um Data Warehouse.    

> **Usado quando**: o ambiente de destino (como Redshift ou Snowflake) é mais rígido e espera dados já organizados.
### 🛠️ ELT (Extract, Load, Transform)
- **Extração**: dados brutos são capturados como no ETL;    
- **Carga**: os dados são primeiramente armazenados em um Data Lake (como o S3);    
- **Transformação**: ocorre posteriormente, usando recursos computacionais do destino.    

> **Usado quando**: temos alta capacidade de processamento no destino (Athena, BigQuery, etc.) e dados são carregados em estado bruto.
## 📌 Carga e Transformação: Detalhamento Técnico
### 📥 Extração
- Pode vir de:    
    - Bancos de dados relacionais (MySQL, PostgreSQL…);        
    - Sistemas como Salesforce, SAP;        
    - APIs REST ou arquivos CSV, logs, JSON;        
- Deve garantir:    
    - **Integridade dos dados** (não perder ou duplicar);        
    - **Política de retries** (caso de falha em API);        
    - **Velocidade adequada** (streaming, near real-time ou batch).        
<div style="page-break-after: always;"></div>
### 🔄 Transformação
Inclui:
- **Limpeza**: remoção de nulos, correção de erros;    
- **Enriquecimento**: junções com outras fontes;    
- **Conversão de tipos**: datas como string para `timestamp`, por exemplo;    
- **Padronização de formatos**;    
- **Cálculos**: agregações, métricas, derivadas;    
- **Validações**: regras de qualidade, como colunas obrigatórias.    

Transformações podem ser feitas em:
- Python com pandas;    
- PySpark em EMR;    
- SQL (no próprio destino, no modelo ELT);    
- Glue, DataBrew ou workflows via Airflow. 
### 📤 Carga
- Armazenamento no destino: S3, Redshift, Snowflake, BigQuery, etc.;    
- Pode ocorrer em **batch** (ex: uma vez por dia) ou **streaming** (ex: Kinesis, Kafka, DMS);    
- Requer:    
    - Monitoramento de falhas;        
    - Garantia de consistência;        
    - Gerenciamento de partições, formatos (Parquet, Avro, etc.).        
## ⚙️ Orquestração do Pipeline
Pipelines devem ser **automatizados e monitorados**. Ferramentas da AWS incluem:
- **AWS Glue**: ETL serverless, com workflows visuais;    
- **Step Functions**: orquestra execução passo a passo;    
- **Lambda + EventBridge**: pipelines reativos a eventos;    
- **MWAA (Apache Airflow gerenciado)**: para pipelines complexos e dependências entre tarefas.    
## 🧠 Conclusão
Um *pipeline de dados bem projetado* é fundamental para sistemas analíticos modernos. Entender quando aplicar ETL ou ELT depende do contexto técnico, tipo de dados, volume, frequência e destino. Dominar essas práticas é essencial para engenheiros de dados que lidam com dados em escala na nuvem.
<div style="page-break-after: always;"></div>

# Fontes de Dados e Formato de Dados Comuns
## 📥 Fontes de Dados Comuns
### 1. *JDBC (Java Database Connectivity)*
- Interface para conectar com bancos relacionais.    
- **Pró:** Independente de plataforma.    
- **Contra:** Requer uso de Java.
### 2. *ODBC (Open Database Connectivity)*
- Interface genérica e independente de linguagem.    
- **Pró:** Compatível com diversas linguagens.    
- **Contra:** Dependente de drivers da plataforma.    
### 3. *APIs (REST/SOAP)*
- Interface comum para sistemas modernos.    
- **Usado quando:** O acesso a dados é feito via HTTP.    
### 4. *Arquivos de Log Brutos*
- Armazenados em S3 ou HDFS.    
- Frequentemente não estruturados.    
### 5. *Streams em Tempo Real*
- Exemplos: **Kafka**, **Kinesis**, **Flink**.    
- Dados chegam em tempo real e requerem sistemas capazes de consumir, processar e armazenar.    
## 📂 Formatos de Dados Comuns
### 1. *CSV (Comma-Separated Values)*
- **Formato:** Texto, legível por humanos.    
- **Prós:** Simples, compatível com quase tudo, ótimo para exportações e importações.    
- **Contras:** Não é eficiente para grandes volumes; difícil lidar com estruturas aninhadas.    
### 2. *JSON (JavaScript Object Notation)*
- **Formato:** Texto, legível por humanos, baseado em pares chave-valor.    
- **Prós:** Suporta estruturas aninhadas e esquema flexível.    
- **Contras:** Menos eficiente que formatos binários.    
<div style="page-break-after: always;"></div>
### 3. *Avro*
- **Formato:** Binário com schema embutido.    
- **Prós:** Eficiente, ideal para streaming e serialização de dados.    
- **Contras:** Não legível por humanos.    
### 4. *Parquet*
- **Formato:** Colunar e binário.    
- **Prós:** Otimizado para leitura seletiva de colunas, compressão eficiente.    
- **Ideal para:** Big data analytics em Spark, Hive, Athena, Redshift Spectrum.    
- **Contras:** Não é legível por humanos; pouco eficiente se você sempre acessa o registro completo. 
## ✅ Outros formatos a considerar:
- ### *ORC (Optimized Row Columnar):*    
    - Similar ao Parquet, mas mais comum em ambientes Hive.        
- ### *XML:*    
    - Ainda usado em integrações legadas, mas com overhead maior.        
- ### *Delta Lake e Iceberg:*    
    - Formatos open table que adicionam controle transacional sobre arquivos Parquet.        
- ### Integração com Serviços AWS:
	- **Athena**: Lê diretamente CSV, JSON, Parquet, ORC.    
	- **Glue**: Converte e cataloga dados em qualquer formato.    
	- **Redshift Spectrum**: Ideal para leitura de Parquet e ORC no S3.
## ✅ Boas práticas:
- **Compactação:** Use gzip, snappy ou zstd com Parquet/Avro.    
- **Schema evolution:** JSON e Avro são mais flexíveis.    
- **Particionamento:** Essencial para desempenho em leitura no S3 (Athena, EMR).
<div style="page-break-after: always;"></div>
# Modelagem de Dados, Linhagem de Dados e Evolução de Modelos (Schema Evolution)
## 📘 Modelagem de Dados (Data Modeling)
### *Pontos abordados no material:*
- Explicação do **modelo dimensional (esquema em estrela)**:    
    - **Fato**: tabela central com métricas quantitativas (ex: compras).        
    - **Dimensões**: tabelas com atributos descritivos (ex: aluno, curso, pagamento).        
    - Uso de **chaves primárias e estrangeiras** para vincular as tabelas.        
    - Representado por **Diagramas Entidade-Relacionamento (ERD)**.       
### *Lacunas e complementos importantes:*
- **Modelo Snowflake**:    
    - Variante do esquema estrela, com dimensões **normalizadas**.        
    - Ajuda a economizar espaço, mas pode dificultar a performance de leitura.        
- **Modelo Relacional vs Dimensional**:    
    - Relacional: usado para OLTP (transacional).        
    - Dimensional: usado para OLAP (analítico).        
- **Modelagem NoSQL**:    
    - Em bancos de documentos (MongoDB, DynamoDB), a modelagem segue o padrão de **desnormalização** para performance.        
    - Conceitos como **modelagem orientada a consultas** são comuns.        
- **Boas práticas**:    
    - Identificar **entidades e relacionamentos**.        
    - Definir **chaves primárias naturais ou substitutas**.        
    - **Evitar redundâncias** e **anomalias de atualização**.        
## 🔁 Linhagem de Dados (Data Lineage)
### *Pontos abordados no material:*
- Representação visual do **fluxo dos dados**, da origem até o destino.    
- Importância para:    
    - **Depuração de erros**.        
    - **Auditoria e compliance**.        
    - **Documentação e onboarding**.        
- Exemplo usando:    
    - **Spline Agent** + **Apache Spark** + **AWS Glue**.        
    - Armazenamento de linhagem no **Amazon Neptune (grafo)**.        
<div style="page-break-after: always;"></div>
### *Lacunas e complementos importantes:*
- **Tipos de linhagem**:    
    - **Linhagem de dados técnica**: descreve jobs, tabelas, scripts, colunas e tipos.        
    - **Linhagem de dados empresarial**: associada a regras de negócios e processos.        
- **Ferramentas populares**:    
    - **OpenLineage**, **Amundsen**, **DataHub**, **Collibra**, **Alation**.        
    - AWS: **AWS Glue Data Catalog**, **SageMaker Lineage**, **Lake Formation**.        
- **Formato de armazenamento da linhagem**:    
    - Geralmente representado como **grafos direcionados acíclicos (DAGs)**.        
- **Integração com orquestradores**:    
    - Airflow pode capturar metadados e gerar linhagem com plug-ins (ex: OpenLineage).        
## 🧬 Evolução de Esquemas (Schema Evolution)
### *Pontos abordados no material:*
- Capacidade de modificar o esquema **sem quebrar pipelines ou sistemas legados**.    
- Suporte a:    
    - **Adição de colunas**.        
    - **Remoção de colunas**.        
    - **Modificação de tipos de dados** (com cautela).        
- Exemplo: **AWS Glue Schema Registry** para registrar e validar esquemas.    
### *Lacunas e complementos importantes:*
- **Compatibilidade de esquema**:    
    - **Forward compatibility**: novos produtores, leitores antigos.        
    - **Backward compatibility**: produtores antigos, leitores novos.        
    - **Full compatibility**: suporta ambos.        
- **Formatos de dados com suporte nativo à evolução**:    
    - **Avro** (com registro de esquema).        
    - **Parquet** (metadados embutidos, mas menos flexível).        
    - **ORC** e formatos transacionais como:        
        - **Apache Hudi**, **Apache Iceberg**, **Delta Lake** (controle de versão + schema evolution).            
- **Exemplos de desafios**:    
    - Alterações de tipo não compatíveis (e.g., `int` → `string`).        
    - Ordem de campos em Avro pode importar se o leitor não for tolerante.        
- **Verificação e validação**:    
    - Glue permite configurar **modo de compatibilidade**.        
    - Integrações com **Kafka + Schema Registry (Confluent ou AWS)**.        
<div style="page-break-after: always;"></div>
## 🧠 Dicas para a prova
- **Atenção a perguntas de compatibilidade de esquemas**.    
- Entender **diferenças entre modelagem OLTP x OLAP**.    
- Reconhecer cenários práticos com ferramentas AWS (Glue, Neptune, S3).    
- Saber distinguir entre **linhagem técnica e de negócios**.    
- Interpretar **impactos de mudanças de esquema** em pipelines ETL.
# Otimização de performance de Bancos de dados
### 1. **Indexação**
- Evita varreduras completas de tabela (full scan).    
- Permite acesso mais rápido aos dados.    
- Ajuda a reforçar **unicidade** e **integridade** dos dados (índices únicos).    
- Fundamental analisar as **consultas** frequentes para definir quais colunas devem ser indexadas.    
- Índices compostos (multi-coluna) podem ser úteis para filtros combinados.    
### 2. **Particionamento**
- Reduz a quantidade de dados lidos ao dividir grandes volumes com base em colunas estratégicas (ex: data, região).    
- Permite:    
    - **Filtragem eficiente** (ex: partição por mês).        
    - **Processamento paralelo** (cada partição processada isoladamente).        
    - **Gerenciamento de ciclo de vida** (partições antigas movidas ou excluídas).        
- Particionamento por intervalo, lista, hash ou híbrido.    
### 3. **Compactação**
- Reduz custo de armazenamento e acelera leitura de disco.    
- Mitiga gargalos de **I/O** de disco.    
- Formatos comuns: **GZIP**, **LZOP**, **BZIP2**, **Zstandard**.    
- Deve-se balancear entre:    
    - **Taxa de compressão** (quanto reduz).        
    - **Tempo de descompressão** (carga de CPU).        
<div style="page-break-after: always;"></div>
### 4. **Compactação Colunar**
- Muito eficiente com dados armazenados em formato colunar (ex: **Parquet**, **ORC**).    
- Vantagem:    
    - Dados homogêneos por coluna facilitam a compressão.        
    - Ideal para workloads de leitura analítica, como em data lakes e data warehouses.        
###  **Complementos Relevantes para a Prova**
#### 🔹 _Query Optimization_
- Use planos de execução (EXPLAIN) para analisar gargalos.    
- Evite `SELECT *` — sempre especifique as colunas necessárias.    
- Prefira filtros com colunas indexadas ou particionadas.    
#### 🔹 _Normalização vs Desnormalização_
- **Normalização** melhora integridade e evita redundância.    
- **Desnormalização** pode melhorar performance de leitura, importante em OLAP.    
#### 🔹 _Materialized Views_
- Armazenam o resultado de queries complexas.    
- Podem ser atualizadas periodicamente (útil para relatórios pesados).    
#### 🔹 _Cache e Buffer Pool_
- Alguns SGBDs fazem cache em memória de dados lidos com frequência (ex: PostgreSQL, Oracle).    
- Dimensionar corretamente a memória alocada ao buffer/cache impacta diretamente na performance.    
#### 🔹 _Tuning de Parâmetros_
- Parâmetros como número de conexões simultâneas, buffers, cache de disco e paralelismo influenciam no desempenho global do banco.
<div style="page-break-after: always;"></div>
# Técnicas de Amostragem de dados
## 🎯 Objetivo da Amostragem
Reduzir o volume de dados a ser processado **mantendo a representatividade** para análise. Ideal quando:
- Se quer testar hipóteses rapidamente;    
- Reduzir custos computacionais;    
- Trabalhar com subconjuntos representativos de dados muito grandes.    
## 📊 *Principais Técnicas Apresentadas*
### 1. *Amostragem Aleatória Simples*
- Cada item tem **chance igual de ser selecionado**.    
- Útil quando **não há categorias distintas** nos dados.    
- Não garante equilíbrio entre subgrupos.    
### 2. *Amostragem Estratificada*
- Dados são divididos em **estratos** (subgrupos homogêneos).    
- É feita uma **amostragem aleatória dentro de cada estrato**.    
- Garante que todos os subgrupos estejam **representados adequadamente**.    
- Exemplo: selecionar 2 exemplos de cada categoria (livros, músicas, roupas etc.), mesmo que desproporcional à população total.    
### 3. *Amostragem Sistêmica*
- Seleção com base em um **intervalo fixo**: ex. "um a cada três".    
- Simples de implementar, mas pode induzir **viés** se houver padrão periódico nos dados.    
### 4. *Outras técnicas citadas:
- **Amostragem por agrupamento (cluster sampling):** divide a população em grupos e sorteia grupos inteiros.    
- **Amostragem por conveniência:** baseada na **facilidade de acesso** aos dados.    
- **Amostragem por julgamento (ou intencional):** baseada no **conhecimento prévio** do analista sobre o que deve ser incluído.    
<div style="page-break-after: always;"></div>
## 🧠 *Conceitos Complementares Relevantes para Certificação*
### 🟧 Técnicas Adicionais:
- **Bootstrap Sampling (reamostragem com reposição):** usada para estimativas estatísticas robustas;    
- **Reservoir Sampling:** útil para amostragem de **fluxos de dados (streams)** com tamanho indeterminado.   
### 🟨 Cuidados ao realizar amostragem:
- Verificar **viés de seleção**;    
- Avaliar **variância e representatividade**;    
- Considerar a **distribuição dos dados originais**.    
### 🟩 Aplicações práticas em Engenharia de Dados:
- Redução de volume para testes de pipelines;    
- Criação de datasets balanceados para ML;    
- Prevenção de overfitting por subamostragem;    
- Validação de dados com consistência entre amostras e população.
# Mecanismos de distorção de dados (Data Skew)
## ✅ *Definição*
Data Skew ocorre quando há **distribuição desigual de dados entre partições** em sistemas distribuídos. Isso afeta negativamente o desempenho de pipelines de processamento distribuído como Spark, Hadoop ou sistemas de banco de dados particionados.
## ⚙️ *Causas Comuns*
1. **Distribuição desigual de chaves**:    
    - Ex: alguns valores de chave (como o ID de Brad Pitt em um banco de filmes) recebem muito mais acessos.        
2. **Problema da celebridade**:    
    - Um pequeno grupo de valores (celebridades) concentra grande volume de dados/tráfego, gerando sobrecarga em partições específicas.        
3. **Particionamento inadequado**:    
    - Estratégias de particionamento genéricas (como hash de ID) não refletem a realidade dos dados.        
4. **Distorção temporal**:    
    - Ex: partições por ano podem gerar partições de tamanhos muito diferentes (anos recentes com muito mais dados).        
<div style="page-break-after: always;"></div>
## 📉 *Impactos*
- Subutilização de nós em um cluster distribuído.    
- Atrasos no processamento por sobrecarga em poucas partições.    
- Custo elevado de computação com baixa performance.    
- Problemas em joins com chaves desbalanceadas.    
## 🛠️ *Técnicas de Mitigação*
1. **Particionamento Adaptativo**:    
    - Reajusta partições dinamicamente com base na distribuição observada.        
2. **Salting**:    
    - Adiciona valor aleatório à chave de partição para melhorar a distribuição (ex: transformar `id` em `id_random_salt`).        
3. **Reparticionamento periódico**:    
    - Reprocessa os dados com nova lógica de particionamento.        
    - Pode ser disruptivo e custoso.        
4. **Amostragem de dados**:    
    - Amostras são usadas para entender a distribuição real dos dados antes do processamento completo.        
5. **Particionamento personalizado**:    
    - Define regras específicas baseadas no conhecimento do domínio (ex: colocar celebridades em partições separadas).        
## 📈 *Monitoramento e Prevenção*
- Use ferramentas como **AWS CloudWatch** para criar **alarmes sobre métricas de distorção**, como:    
    - Tempo de execução de etapas.        
    - Volume de dados por partição.        
    - Tamanho de arquivos em partições.        
<div style="page-break-after: always;"></div>
## 🧠 *Complementos Importantes para a Certificação*
Embora o material original seja abrangente, alguns pontos extras úteis incluem:

|Conceito|Detalhe|
|---|---|
|**Skew em joins**|Quando uma chave de join é muito frequente, o nó que processa essa chave pode ficar sobrecarregado. Técnicas como **broadcast join** ou **salting + explode** podem ser usadas.|
|**Uso de `salting` com Spark**|Pode-se usar `withColumn("key_salted", concat(col("key"), lit("_"), rand()))` antes de um join.|
|**Shuffle Skew**|Distorção durante a movimentação de dados entre nós. Indica necessidade de reavaliar chave de partição.|
|**Ferramentas de diagnóstico**|Spark UI, logs de execução e métricas de jobs ajudam a identificar gargalos causados por skew.|
|**Split de arquivos**|Arquivos muito grandes ou muito pequenos também podem causar skew se mal balanceados no HDFS/S3.|

# Perfil e Validação de Dados
## 1. *Definição Geral*
Perfil de dados e validação dizem respeito à **qualidade dos dados** e sua **adequação ao uso**, garantindo que os dados sejam confiáveis, completos, consistentes e precisos para análises e tomadas de decisão.
## 🧪 *Dimensões da Validação de Dados*
### 📌 **1. Integridade de Dados**
- Refere-se à presença de dados esperados:    
    - Verificação de **dados faltantes (nulls)**.        
    - Cálculo da **percentual de campos ausentes** por coluna.        
    - Exemplo: valores salariais ausentes afetam médias calculadas.        
🔧 **Ações recomendadas:**
- **Remoção** de registros incompletos.    
- **Imputação** com valores padrão, média ou mediana.    
- Avaliar se o **dado ausente é crítico ou tolerável**.    
<div style="page-break-after: always;"></div>
### 📌 **2. Consistência de Dados**
- Dados devem manter **formato e representação uniformes** entre diferentes tabelas/fontes.    
- Exemplo clássico:    
    - Escalas de avaliação de filmes: 1 a 5 vs. 1 a 10.        
    - Unidades de medida diferentes (ex: "kg" vs. "g").        
🔧 **Ações recomendadas:**
- **Padronização de formatos e unidades** antes de combinar dados.    
- **Validação cruzada** entre campos relacionados (ex: tipo de dado, range, categoria).    
### 📌 **3. Precisão dos Dados**
- Verifica se os dados são **corretos e confiáveis**.    
- Difícil de validar sem uma **fonte de verdade (golden source)**.    
🔧 **Ações recomendadas:**
- **Checks de sanidade** (valores dentro de faixas aceitáveis).    
- **Verificações estatísticas** de distribuições esperadas.    
- Uso de **modelos de detecção de outliers/anomalias**.    
#### 📌 **4. Validade / Integridade Referencial**
- Avalia se os dados **mantêm relações entre si ao longo do tempo**.    
- Foco em:    
    - **Chaves primárias e estrangeiras**.        
    - Relacionamentos entre tabelas (**joins consistentes**).        
    - **Atualizações que quebram dependências**.        
🔧 **Ações recomendadas:**
- **Verificação periódica de integridade referencial**.    
- Garantia de **cascata de atualizações e deleções**.    
- Ferramentas de **data lineage** para rastrear dependências.    
## ➕ *Complementos Relevantes para Certificação*
### 📊 **Perfil de Dados (Data Profiling)**
- Processo de análise de dados brutos para entender sua **estrutura, qualidade e conteúdo**.    
- Métricas comuns:    
    - **Cardinalidade** (nº de valores distintos por campo).        
    - **Frequência de valores**.        
    - **Distribuição estatística**.        
    - **Análise de padrões (regex, formatos esperados)**.        
🛠️ **Ferramentas úteis**:
- AWS Glue DataBrew, Great Expectations, Deequ, Pandera, PyDeequ.    
<div style="page-break-after: always;"></div>
### 🧠 **Boas Práticas**
- Automatizar validações com pipelines de dados (ETL/ELT).    
- Integrar verificações de qualidade com alertas e dashboards.    
- Implementar **testes unitários e de integração** para dados.    
- Manter **versionamento de esquema e contratos de dados (Data Contracts)**.    
## 📌 Conclusão
A validação e o perfilamento de dados são **fundamentais para garantir a confiabilidade e o valor analítico** em um pipeline de engenharia de dados. Em contextos de certificação, espere perguntas sobre:
- Identificação de problemas comuns de qualidade.    
- Estratégias para validação e profilamento.    
- Impacto da má qualidade nos resultados analíticos.
# Revisão de SQL
## Agregação, Agrupamento, Ordenação e Pivoteamento
## Tipos de Junção (Join Types)
## Expressões Regulares
# Versionamento (GIT)
## 📌 *Conceitos Fundamentais*
- Git é um **sistema de controle de versão distribuído**.    
- Utiliza **repositórios locais e remotos** (ex: GitHub).    
- Permite **trabalho colaborativo simultâneo**, sem conflitos frequentes.    
- Suporte a **ramificações (branches)** para desenvolvimento de features, correções e testes.    
## 📌 *Fluxo de Trabalho Básico*
1. `git clone`: Clona um repositório remoto.    
2. `git add`: Adiciona mudanças para a área de staging.    
3. `git commit -m`: Salva as alterações localmente com uma mensagem.    
4. `git push`: Envia as alterações ao repositório remoto.    
5. `git pull`: Atualiza a branch local com alterações do remoto.    
6. `git branch`, `git checkout -b`, `git merge`: Criação, navegação e integração de branches.    
## 📌 *Configuração e Inicialização*
- `git init`: Cria um novo repositório local.    
- `git config`: Configura nome de usuário e e-mail.    
## 📌 *Diagnóstico e Histórico*
- `git status`: Mostra estado dos arquivos.    
- `git log`: Histórico de commits.    
- `git diff`: Diferença entre versões.    
- `git blame`: Identifica quem alterou o quê.    
### 📌 *Gerenciamento de Branches*
- `git branch`: Lista branches locais.    
- `git branch -d`: Deleta branch local.    
- `git checkout`: Muda de branch.    
- `git checkout -b`: Cria e muda para nova branch.    
- `git merge`: Mescla uma branch à branch atual.    
- `git rebase`: Reaplica commits de uma branch sobre outra.    
- `git cherry-pick`: Aplica commit específico a outra branch.    
## 📌 *Comandos Avançados*
- `git stash`, `git stash pop`: Armazenamento temporário de alterações não commitadas.    
- `git reset [--hard]`: Redefine staging ou diretório de trabalho.    
- `git revert`: Desfaz alterações de um commit anterior.    
- `git fetch`: Busca alterações do remoto sem aplicar (diferente do pull).    
- `git remote add/list`: Adiciona e lista repositórios remotos.    
## 📌 *Manutenção e Recuperação*
- `git fsck`: Verifica integridade do repositório.    
- `git gc`: Limpeza e otimização (garbage collection).    
- `git reflog`: Histórico de atualizações das referências (útil para recuperar commits perdidos).    
## 🧠 *Pontos Relevantes que Poderiam Ser Complementados para a Certificação*
Apesar do documento ser bastante abrangente, **alguns conceitos úteis para certificação não foram cobertos**, como:
### 📁 **.gitignore**
- Arquivo que lista padrões de arquivos e pastas a serem ignorados pelo Git.    
- Exemplo prático: `*.log`, `venv/`, `__pycache__/`.    
### 🔄 **Workflows de Git**
- **Git Flow**, **Feature Branch Workflow**, **Trunk-based development**.    
- Importante para equipes de engenharia de dados em ambientes colaborativos.    
### 🔐 **Controle de Acesso e Segurança**
- Chaves SSH vs autenticação com token pessoal (PAT).    
- Importante ao trabalhar com repositórios privados.    
### 🔄 **CI/CD Integrado**
- Git com **pipelines de integração contínua** (GitHub Actions, GitLab CI).    
- Ajuda a automatizar testes, deploy de ETLs e versionamento de DAGs (Airflow, por exemplo).    
### 📦 **Versionamento de Dados**
- Embora o Git versiona **código**, para dados pode-se usar:    
    - [DVC (Data Version Control)](https://dvc.org/)        
    - LakeFS, Delta Lake, Apache Hudi (com suporte a controle de versões de dados)        
### 🔄 **Tagging e Releases**
- `git tag`: Marca versões específicas (útil para deploys).    
- `git describe`: Mostra a versão associada ao último tag.
## 🎯 *O Que Saber Para a Certificação de Engenharia de Dados*
1. **Comandos essenciais do Git** (clone, pull, push, branch, merge, etc.).    
2. **Uso de branches** para colaboração segura.    
3. **Fluxo básico de staging → commit → push**.    
4. **Histórico e reversões de mudanças**.    
5. **Boa prática com `.gitignore` e tags**.    
6. **Como repositórios locais/remotos interagem**.    
7. **Como o versionamento colabora com dados e pipelines (Git + CI/CD + DVC/LakeFS)**.
