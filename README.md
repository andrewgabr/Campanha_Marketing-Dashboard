# Dashboard da Campanha de Marketing

Este é o dashboard para monitoramento e análise da campanha de marketing, fornecendo insights sobre dados de clientes e produtos. Ele utiliza informações de várias fontes para gerar visualizações que auxiliam nas decisões de marketing.

![Imagem do Dashboard](caminho/para/imagem-do-dashboard.jpg)

## Funcionalidades

- Análise de clientes por faixa etária, status profissional e faixa de renda.
- Visualização do desempenho de produtos, analisando a idade do modelo do produto.
- Ferramentas de visualização de dados para facilitar a interpretação e tomada de decisões.

## Queries Utilizadas

### Query 01: Análise de Cliente

Esta query agrupa os clientes por faixa etária, status profissional e faixa de renda, considerando a data de nascimento e a renda dos clientes.

```sql
--query 01
select t2.gender, t1.professional_status, 
CASE 
WHEN date_part('year',AGE(current_date ,t1.birth_date)) < 20 then '0-20' 
when date_part('year',AGE(current_date ,t1.birth_date)) BETWEEN 20 and 40 then '20-40' 
when date_part('year',AGE(current_date ,t1.birth_date)) BETWEEN 40 and 60 then '40-60' 
when date_part('year',AGE(current_date ,t1.birth_date)) BETWEEN 20 and 40 then '60-80' 
ELSE '+80' end as age,
CASE 
WHEN t1.income <= 5000 then '0-5k'
WHEN t1.income BETWEEN 5000 and 10000 then '5-10k'
WHEN t1.income BETWEEN 10000 and 15000 then '10-15k'
WHEN t1.income BETWEEN 15000 and 20000 then '15-20k'
else '+20000'
end as income
from sales.customers as t1
left join temp_tables.ibge_genders as t2
on t2.first_name = t1.first_name
