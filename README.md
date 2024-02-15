## Создание таблиц
```
create table customer (
    customer_id int4,
    first_name varchar(50),
    last_name varchar(50),
    gender varchar(30),
    DOB varchar(50), 
    job_title varchar(50),
    job_industry_category varchar(50),
    wealth_segment varchar(50),
    deceased_indicator varchar(50),
    owns_car varchar(30),
    address varchar(50),
    postcode varchar(30),
    state varchar(30),
    country varchar(30),
    property_valuation int4
);
```
![Alt text](/imgs/customer1.png?raw=true "Optional Title")
![Alt text](/imgs/customer2.png?raw=true "Optional Title")
```
create table transaction (
	transaction_id int4,
	product_id int4, 	
	customer_id int4, 
	transaction_date varchar(30),  
	online_order varchar(30), 
	order_status varchar(30), 
	brand varchar(30),
	product_line varchar(30), 
	product_class varchar(30),
	product_size varchar(30), 
	list_price float4, 
	standard_cost float4 
)

```
![Alt text](/imgs/transaction.png?raw=true "Optional Title")

## Выполнение запросов

### (1 балл) Вывести все уникальные бренды, у которых стандартная стоимость выше 1500 долларов.

```
select distinct brand
from "transaction" t 
where standard_cost  > 1500;
```
![Alt text](/imgs/task1.png?raw=true "Optional Title")


### (1 балл) Вывести все подтвержденные транзакции за период '2017-04-01' по '2017-04-09' включительно.
Сперва приведем поле "transaction_date" к типу date:
```
alter  table "transaction" 
alter column transaction_date type date using TO_DATE(transaction_date,'DD.MM.YYYY')
```
Теперь можно использовать оператор "between" для этого поля:
```
select *
from "transaction" t 
where order_status = 'Approved' and (transaction_date  between '2017-04-01' and '2017-04-09')
```
![Alt text](/imgs/task2.png?raw=true "Optional Title")

### (1 балл) Вывести все профессии у клиентов из сферы IT или Financial Services, которые начинаются с фразы 'Senior'.

```
select job_title
from customer c 
where (job_industry_category = 'IT' or job_industry_category  = 'Financial Services') and job_title  like 'Senior%'
```
![Alt text](/imgs/task3.png?raw=true "Optional Title")

### (1 балл) Вывести все бренды, которые закупают клиенты, работающие в сфере Financial Services

```
select distinct brand
from "transaction" t 
join customer c 
on t.customer_id  = c.customer_id 
where c.job_industry_category  = 'Financial Services'
```
![Alt text](/imgs/task4.png?raw=true "Optional Title")

### (1 балл) Вывести 10 клиентов, которые оформили онлайн-заказ продукции из брендов 'Giant Bicycles', 'Norco Bicycles', 'Trek Bicycles'.

```
select c.*
from customer c 
join "transaction" t 
on c.customer_id = t.customer_id 
where t.online_order = 'True' and brand in ('Giant Bicycles', 'Norco Bicycles', 'Trek Bicycles')
limit 10
```
![Alt text](/imgs/task5.1.png?raw=true "Optional Title")
![Alt text](/imgs/task5.2.png?raw=true "Optional Title")

### (1 балл) Вывести всех клиентов, у которых нет транзакций.

```
select distinct c.*
from customer c 
full outer join "transaction" t 
on c.customer_id  = t.customer_id 
where t.customer_id is null and c.customer_id is not null
```
![Alt text](/imgs/task6.1.png?raw=true "Optional Title")
![Alt text](/imgs/task6.2.png?raw=true "Optional Title")

### (2 балла) Вывести всех клиентов из IT, у которых транзакции с максимальной стандартной стоимостью.

```
select distinct c.*
from customer c 
join "transaction" t 
on c.customer_id  = t.customer_id 
where c.job_industry_category = 'IT' and t.standard_cost = (select max(standard_cost) from "transaction")
```
![Alt text](/imgs/task7.1.png?raw=true "Optional Title")
![Alt text](/imgs/task7.2.png?raw=true "Optional Title")

### (2 балла) Вывести всех клиентов из сферы IT и Health, у которых есть подтвержденные транзакции за период '2017-07-07' по '2017-07-17'.

```
select distinct c.*
from customer c 
join "transaction" t 
on c.customer_id  = t.customer_id 
where c.job_industry_category  in ('IT', 'Health') and order_status = 'Approved' and transaction_date between '2017-07-07' and '2017-07-17'
```
![Alt text](/imgs/task8.1.png?raw=true "Optional Title")
![Alt text](/imgs/task8.2.png?raw=true "Optional Title")
