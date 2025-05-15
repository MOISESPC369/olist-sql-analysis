# Análise do Dataset Olist com SQL

## Objetivo do Projeto  
Analisar dados reais de e-commerce da base Olist, explorando pedidos, clientes, produtos, vendedores e entregas para extrair insights importantes usando SQL.

## Descrição do Dataset  
O dataset contém informações sobre clientes, pedidos, itens, pagamentos, avaliações, produtos, vendedores e localização geográfica.

## Tabelas e Quantidade de Registros

| Tabela                           | Total de Linhas |
|----------------------------------|-----------------|
| olist_customers_dataset          | 99.441          |
| olist_orders_dataset             | 99.441          |
| olist_order_items_dataset        | 112.650         |
| olist_order_payments_dataset     | 103.886         |
| olist_order_reviews_dataset      | 99.441          |
| olist_products_dataset           | 32.951          |
| olist_sellers_dataset            | 3.095           |
| olist_geolocation_dataset        | 1.000.163       |
| product_category_name_translation| 71              |

## Principais Queries

### 1. Quais estados mais compram?  
```sql
SELECT customer_state, COUNT(*) AS total_pedidos
FROM olist_orders_dataset o
JOIN olist_customers_dataset c ON o.customer_id = c.customer_id
GROUP BY customer_state
ORDER BY total_pedidos DESC
LIMIT 10;
```

### 2. Quais categorias têm mais pedidos?
```sql
SELECT p.product_category_name, COUNT(*) AS total_pedidos
FROM olist_order_items_dataset oi
JOIN olist_products_dataset p ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY total_pedidos DESC
LIMIT 10;
```

### 3. Vendedores com mais atrasos (baseado em datas de entrega):
```sql
CREATE OR REPLACE VIEW view_sellers_atrasos AS
SELECT s.seller_id,
       COUNT(*) AS total_atrasos
FROM olist_order_items_dataset oi
JOIN olist_sellers_dataset s ON oi.seller_id = s.seller_id
JOIN olist_orders_dataset o ON oi.order_id = o.order_id
WHERE o.order_delivered_customer_date > o.order_estimated_delivery_date
GROUP BY s.seller_id;
```
