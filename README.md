
# 📊 Projeto de Análise de Demonstrativos Contábeis - ANS

[![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)](https://github.com/ThiagoJoestar/Teste-de-API)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-ThiagoPiassi-blue?logo=linkedin)](https://linkedin.com/in/thiagopiassi)

Este repositório contém a estrutura inicial de um projeto SQL focado na ingestão e análise dos demonstrativos contábeis e dados cadastrais de operadoras registradas na ANS (Agência Nacional de Saúde Suplementar).

> 🚧 **Projeto em desenvolvimento**. Em breve, este repositório contará com scripts completos, visualizações analíticas e documentação detalhada.

---

## 🧠 Objetivo

Criar um pipeline simples para ingestão de dados públicos da ANS, estruturando tabelas SQL e realizando análises básicas de desempenho financeiro das operadoras.

---

## 📁 Estrutura Inicial

### ✅ Etapas de Preparação

1. **Download dos Arquivos**
   - Baixar os demonstrativos contábeis dos últimos **2 anos**.
   - Baixar os dados cadastrais das **operadoras ativas** em formato CSV.

---

## 🧩 Scripts SQL

### 1. 📦 Criação de Tabelas

```sql
-- Tabela para Dados Cadastrais das Operadoras
CREATE TABLE operadoras (
    id_operadora INT PRIMARY KEY,
    nome VARCHAR(255),
    cnpj VARCHAR(18),
    registro_ans VARCHAR(20),
    data_registro DATE
);

-- Tabela para Demonstrativos Contábeis
CREATE TABLE demonstrativos_contabeis (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_operadora INT,
    ano INT,
    trimestre INT,
    despesa_eventos_sinistros DECIMAL(15, 2),
    FOREIGN KEY (id_operadora) REFERENCES operadoras(id_operadora)
);
```

---

### 2. 📥 Importação de Dados

```sql
-- Importar Dados Cadastrais
LOAD DATA INFILE 'caminho/para/operadoras.csv'
INTO TABLE operadoras
FIELDS TERMINATED BY ';'
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_operadora, nome, cnpj, registro_ans, data_registro);
```

```sql
-- Importar Demonstrativos Contábeis
LOAD DATA INFILE 'caminho/para/demonstrativos_contabeis.csv'
INTO TABLE demonstrativos_contabeis
FIELDS TERMINATED BY ';'
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_operadora, ano, trimestre, despesa_eventos_sinistros);
```

---

### 3. 📊 Consultas Analíticas

```sql
-- Top 10 operadoras com maiores despesas no último trimestre
SELECT o.nome, SUM(d.despesa_eventos_sinistros) AS total_despesas
FROM demonstrativos_contabeis d
JOIN operadoras o ON d.id_operadora = o.id_operadora
WHERE d.ano = YEAR(CURDATE()) AND d.trimestre = QUARTER(CURDATE()) - 1
GROUP BY o.nome
ORDER BY total_despesas DESC
LIMIT 10;
```

```sql
-- Top 10 operadoras com maiores despesas no último ano
SELECT o.nome, SUM(d.despesa_eventos_sinistros) AS total_despesas
FROM demonstrativos_contabeis d
JOIN operadoras o ON d.id_operadora = o.id_operadora
WHERE d.ano = YEAR(CURDATE()) - 1
GROUP BY o.nome
ORDER BY total_despesas DESC
LIMIT 10;
```

---

## 🔗 Contato

👨‍💻 [Thiago Piassi no LinkedIn](https://linkedin.com/in/thiagopiassi)  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-ThiagoPiassi-blue?logo=linkedin&style=flat-square)](https://linkedin.com/in/thiagopiassi)





