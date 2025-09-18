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

-- 1. Ver os primeiros registros
SELECT * FROM credito LIMIT 10;

-- 2. Contar o total de registros
SELECT COUNT(*) FROM credito;

-- 3. Ver estrutura da tabela
DESCRIBE credito;

-- 4. Listar valores únicos de escolaridade
SELECT DISTINCT escolaridade FROM credito;

-- 5. Contar usuários por salário
SELECT COUNT(*), salario_anual FROM credito GROUP BY salario_anual;

-- 6. Contar usuários por sexo
SELECT COUNT(*), sexo FROM credito GROUP BY sexo;

-- 7. Clientes com maior limite de crédito por escolaridade e tipo_cartao
SELECT MAX(limite_credito) AS limite_credito, escolaridade, tipo_cartao, sexo
FROM credito
WHERE escolaridade != 'na' AND tipo_cartao != 'na'
GROUP BY escolaridade, tipo_cartao, sexo
ORDER BY limite_credito DESC
LIMIT 10;

-- 8. Média e soma do valor gasto por sexo
SELECT AVG(valor_transacoes_12m) AS media_valor_gasto,
       SUM(valor_transacoes_12m) AS soma_valor_gasto,
       sexo
FROM credito
GROUP BY sexo;

-- 9. Relação entre produtos, transações e limite de crédito
SELECT AVG(qtd_produtos) AS qtd_produtos,
       AVG(valor_transacoes_12m) AS media_valor_transacoes,
       AVG(limite_credito) AS media_limite
FROM credito
WHERE escolaridade != 'na'
GROUP BY sexo, salario_anual
ORDER BY AVG(valor_transacoes_12m) DESC;

