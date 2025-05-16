# Домашнее задание к занятию «SQL. Часть 2» - Крюков Егор


### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Решение 
Нужно выполнить такой запрос

```sql
SELECT 
    s.store_id,
    CONCAT(st.last_name, ' ', st.first_name) AS staff_name,
    c.city AS store_city,
    COUNT(cu.customer_id) AS customer_count
FROM 
    store s
JOIN 
    staff st ON s.manager_staff_id = st.staff_id
JOIN 
    address a ON s.address_id = a.address_id
JOIN 
    city c ON a.city_id = c.city_id
JOIN 
    customer cu ON s.store_id = cu.store_id
GROUP BY 
    s.store_id, st.staff_id, c.city_id
HAVING 
    COUNT(cu.customer_id) > 300;
```
В ответ получим такую таблицу с данными

| store_id | staff_name   | store_city | customer_count |
|----------|--------------|------------|----------------|
|        1 | Hillyer Mike | Lethbridge |            326 |


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 
Нужно выполнить такой запрос

```sql
SELECT COUNT(*) AS films_above_average_length
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

В ответ получим такую таблицу с данными

| films_above_average_length |
|----------------------------|
|                        489 |

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение 

Запрос

```sql
SELECT 
    DATE_FORMAT(payment_date, '%Y-%m') AS payment_month,
    SUM(amount) AS total_payments,
    COUNT(DISTINCT rental_id) AS rental_count
FROM 
    payment
GROUP BY 
    payment_month
ORDER BY 
    total_payments DESC
LIMIT 1;
```

В ответ получим такую таблицу с данными

| payment_month | total_payments | rental_count |
|---------------|----------------|--------------|
| 2005-07       |       28368.91 |         6709 |
