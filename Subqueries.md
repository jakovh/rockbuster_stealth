A query which retrieves average amount paid by top 5 customers
```
SELECT AVG(total_amount_paid.total_amount_paid) AS average_amount_paid
FROM (SELECT A.customer_id AS customer_id,
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
      ORDER BY total_amount_paid DESC LIMIT 5) AS total_amount_paid

```

A query which answers the question on how many of the top 5 customers are based within each country
```
SELECT D.country AS country,
       COUNT(A.customer_id) AS all_customer_count,
       COUNT(DISTINCT top_5) AS top_5_customers
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
LEFT JOIN
      (SELECT A.customer_id AS customer_id,
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
      ORDER BY total_amount_paid DESC LIMIT 5) AS top_5
ON D.country = top_5.country
GROUP BY D.country
HAVING COUNT(DISTINCT top_5) > 0
ORDER BY all_customer_count DESC
```
