# Projeto SQL - EBAC (Base de Crédito)

Este projeto faz parte do **módulo de SQL** do curso de Analista de Dados da **EBAC**.  
O objetivo é praticar a criação de tabelas, consultas e análise exploratória de dados utilizando o **Amazon Athena** e arquivos armazenados no **S3**.

---

## 📂 Estrutura do projeto
- `sql/create_table.sql`: Script para criação da tabela `credito`.
- `sql/queries.sql`: Conjunto de queries de análise.
- `docs/guia_sql_credito.pdf`: Guia em PDF com instruções detalhadas.

---

## 🏗️ Criação da Tabela

A tabela `credito` foi criada a partir do arquivo `credito8.csv`, com a seguinte estrutura:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.credito (
    id INT,
    sexo STRING,
    dependentes INT,
    escolaridade STRING,
    estado_civil STRING,
    salario_anual STRING,
    tipo_cartao STRING,
    limite_credito FLOAT,
    valor_transacoes_12m FLOAT,
    qtd_transacoes_12m INT
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = ',',
  'field.delim' = ','
)
LOCATION 's3://SEU-BUCKET/credito/'
TBLPROPERTIES ('has_encrypted_data'='false');


<img width="1107" height="343" alt="image" src="https://github.com/user-attachments/assets/bfb109ea-5309-47a7-86d9-14993534a737" />
