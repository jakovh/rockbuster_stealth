A query which retrieves top 10 countries by customers

```
SELECT D.country AS country,
       COUNT(A.customer_id) AS number_of_customers
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
GROUP BY country
ORDER BY number_of_customers DESC LIMIT 10
```


A query which retrieves top 10 cities within top 10 countries by customers
```
SELECT D.country AS country,
       C.city AS city,
       COUNT(A.customer_id) AS number_of_customers
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
WHERE country IN ('India', 'China', 'United States', 'Japan', 'Mexico', 'Brazil', 'Russian
Federation', 'Philippines', 'Turkey', 'Indonesia')
GROUP BY country, city
ORDER BY number_of_customers DESC LIMIT 10
```

A query which retrieves top 5 customers within top 10 cities by total amount paid to Rockbuster Stealth LLC
```
SELECT A.customer_id AS customer_id,
       B.first_name AS first_name,
       B.last_name AS last_name,
       E.country AS country,
       D.city AS city,
       SUM(A.amount) AS total_amount_paid
FROM payment A
INNER JOIN customer B ON A.customer_id = B.customer_id
INNER JOIN address C ON B.address_id = C.address_id
INNER JOIN city D ON C.city_id = D.city_id
INNER JOIN country E ON D.country_id = E.country_id
WHERE D.city IN ('Ambattur', 'Citrus Heights', 'Aurora', 'Iwaki', 'Acua', 'Tianjin', 'Teboksary',
'So Leopoldo', 'Shanwei', 'Cianjur')
GROUP BY A.customer_id, B.first_name, B.last_name, E.country, D.city
ORDER BY total_amount_paid DESC LIMIT 5
```
