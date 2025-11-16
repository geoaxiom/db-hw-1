# Домашнее задание 1. Создание и нормализация базы данных

### 1. Загрузить файл с данными по клиентам и транзакциям.

<img width="1116" height="820" alt="img_01" src="https://github.com/user-attachments/assets/68ce0ae2-43bd-4672-8d4f-1c30e45b23ec" />

### 1.1 Переименуем таблици с изначальными данными для последующей работы

<img width="616" height="107" alt="img_02" src="https://github.com/user-attachments/assets/d7d09c13-df9e-4c2a-87b8-20ccc652b885" />

### 2. Продумать структуру базы данных и отрисовать в редакторе.

```
CREATE TABLE state (
    state_id INT PRIMARY KEY,
    state_name VARCHAR(50)
);

CREATE TABLE country (
    country_id INT PRIMARY KEY,
    country_name VARCHAR(50)
);

CREATE TABLE address (
    address_id INT PRIMARY KEY,
    address VARCHAR(255),
    postcode VARCHAR(20),
    state_id INT,
    country_id INT,
    FOREIGN KEY (state_id) REFERENCES state(state_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE gender (
    gender_id INT PRIMARY KEY,
    gender_name VARCHAR(255)
);

CREATE TABLE job (
    job_id INT PRIMARY KEY,
    job_title VARCHAR(255)
);

CREATE TABLE industry_category (
    industry_category_id INT PRIMARY KEY,
    industry_category_name VARCHAR(255)
);

CREATE TABLE wealth_segment (
    wealth_segment_id INT PRIMARY KEY,
    wealth_segment_name VARCHAR(255)
);

CREATE TABLE property_valuation (
    property_valuation_id INT PRIMARY KEY,
    property_valuation_value INT
);

CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    gender_id INT,
    DOB DATE,
    deceased_indicator BOOLEAN,
    owns_car BOOLEAN,
    address_id INT,
    job_id INT,
    industry_category_id INT,
    wealth_segment_id INT,
    property_valuation_id INT,
    FOREIGN KEY (gender_id) REFERENCES gender(gender_id),
    FOREIGN KEY (address_id) REFERENCES address(address_id),
    FOREIGN KEY (job_id) REFERENCES job(job_id),
    FOREIGN KEY (industry_category_id) REFERENCES industry_category(industry_category_id),
    FOREIGN KEY (wealth_segment_id) REFERENCES wealth_segment(wealth_segment_id),
    FOREIGN KEY (property_valuation_id) REFERENCES property_valuation(property_valuation_id)
);

CREATE TABLE transaction (
    transaction_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT,
    order_id INT,
    transaction_date DATE,
    FOREIGN KEY (order_id) REFERENCES "order"(order_id),
    FOREIGN KEY (product_id) REFERENCES product(product_id),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);

CREATE TABLE "order" (
    order_id INT PRIMARY KEY,
    online_order BOOLEAN,
    order_status VARCHAR(50)
);

CREATE TABLE ref_product_order (
    order_id INT,
    product_id INT,
    FOREIGN KEY (product_id) REFERENCES product(product_id),
    FOREIGN KEY (order_id) REFERENCES "order"(order_id)
);

CREATE TABLE product (
    product_id INT PRIMARY KEY,
    product_line VARCHAR(50),
    product_class VARCHAR(50),
    brand VARCHAR(100),
    product_size VARCHAR(50)
);

CREATE TABLE product_price (
    product_price_id INT PRIMARY KEY,
    product_id INT,
    list_price DECIMAL(10, 2),
    standard_cost DECIMAL(10, 2),
    price_date DATE,
    FOREIGN KEY (product_id) REFERENCES product(product_id)
);

```

<img width="971" height="1129" alt="all_tables" src="https://github.com/user-attachments/assets/f9003d04-dfb4-4231-85b1-7d54936f808b" />

### 3. Нормализовать базу данных (от 1НФ до 3НФ), описав, к какой нормальной форме приводится таблица и почему таблица в этой нормальной форме изначально не находилась.
Плоские таблици витрин `tmp_transaction` и `tmp_customer` изначально находились в 1NF

### 4. Создать все таблицы в DBeaver, указав первичные ключи к таблицам, правильные типы данных, могут ли поля быть пустыми или нет (использовать команду CREATE TABLE).
```
-- public.country definition

-- Drop table

-- DROP TABLE public.country;

CREATE TABLE public.country (
	country_id int4 NOT NULL,
	country_name varchar(50) NULL,
	CONSTRAINT country_pkey PRIMARY KEY (country_id)
);


-- public.gender definition

-- Drop table

-- DROP TABLE public.gender;

CREATE TABLE public.gender (
	gender_id int4 NOT NULL,
	gender_name varchar(255) NULL,
	CONSTRAINT gender_pkey PRIMARY KEY (gender_id)
);


-- public.industry_category definition

-- Drop table

-- DROP TABLE public.industry_category;

CREATE TABLE public.industry_category (
	industry_category_id int4 NOT NULL,
	industry_category_name varchar(255) NULL,
	CONSTRAINT industry_category_pkey PRIMARY KEY (industry_category_id)
);


-- public.job definition

-- Drop table

-- DROP TABLE public.job;

CREATE TABLE public.job (
	job_id int4 NOT NULL,
	job_title varchar(255) NULL,
	CONSTRAINT job_pkey PRIMARY KEY (job_id)
);


-- public."order" definition

-- Drop table

-- DROP TABLE public."order";

CREATE TABLE public."order" (
	order_id int4 NOT NULL,
	online_order bool NULL,
	order_status varchar(50) NULL,
	CONSTRAINT order_pkey PRIMARY KEY (order_id)
);


-- public.product definition

-- Drop table

-- DROP TABLE public.product;

CREATE TABLE public.product (
	product_id int4 NOT NULL,
	product_line varchar(50) NULL,
	product_class varchar(50) NULL,
	brand varchar(100) NULL,
	product_size varchar(50) NULL,
	CONSTRAINT product_pkey PRIMARY KEY (product_id)
);


-- public.property_valuation definition

-- Drop table

-- DROP TABLE public.property_valuation;

CREATE TABLE public.property_valuation (
	property_valuation_id int4 NOT NULL,
	property_valuation_value int4 NULL,
	CONSTRAINT property_valuation_pkey PRIMARY KEY (property_valuation_id)
);


-- public.state definition

-- Drop table

-- DROP TABLE public.state;

CREATE TABLE public.state (
	state_id int4 NOT NULL,
	state_name varchar(50) NULL,
	CONSTRAINT state_pkey PRIMARY KEY (state_id)
);


-- public.tmp_customer definition

-- Drop table

-- DROP TABLE public.tmp_customer;

CREATE TABLE public.tmp_customer (
	customer_id int4 NULL,
	first_name varchar(100) NULL,
	last_name varchar(100) NULL,
	gender varchar(50) NULL,
	dob date NULL,
	job_title varchar(100) NULL,
	job_industry_category varchar(100) NULL,
	wealth_segment varchar(100) NULL,
	deceased_indicator varchar(10) NULL,
	owns_car bool NULL,
	address varchar(200) NULL,
	postcode varchar(50) NULL,
	state varchar(100) NULL,
	country varchar(100) NULL,
	property_valuation int4 NULL
);


-- public.tmp_transaction definition

-- Drop table

-- DROP TABLE public.tmp_transaction;

CREATE TABLE public.tmp_transaction (
	transaction_id int4 NULL,
	product_id int4 NULL,
	customer_id int4 NULL,
	transaction_date date NULL,
	online_order bool NULL,
	order_status varchar(50) NULL,
	brand varchar(100) NULL,
	product_line varchar(50) NULL,
	product_class varchar(50) NULL,
	product_size varchar(50) NULL,
	list_price varchar(50) NULL,
	standard_cost varchar(50) NULL
);


-- public.wealth_segment definition

-- Drop table

-- DROP TABLE public.wealth_segment;

CREATE TABLE public.wealth_segment (
	wealth_segment_id int4 NOT NULL,
	wealth_segment_name varchar(255) NULL,
	CONSTRAINT wealth_segment_pkey PRIMARY KEY (wealth_segment_id)
);


-- public.address definition

-- Drop table

-- DROP TABLE public.address;

CREATE TABLE public.address (
	address_id int4 NOT NULL,
	address varchar(255) NULL,
	postcode varchar(20) NULL,
	state_id int4 NULL,
	country_id int4 NULL,
	CONSTRAINT address_pkey PRIMARY KEY (address_id),
	CONSTRAINT address_country_id_fkey FOREIGN KEY (country_id) REFERENCES public.country(country_id),
	CONSTRAINT address_state_id_fkey FOREIGN KEY (state_id) REFERENCES public.state(state_id)
);


-- public.customer definition

-- Drop table

-- DROP TABLE public.customer;

CREATE TABLE public.customer (
	customer_id int4 NOT NULL,
	first_name varchar(255) NULL,
	last_name varchar(255) NULL,
	gender_id int4 NULL,
	dob date NULL,
	deceased_indicator bool NULL,
	owns_car bool NULL,
	address_id int4 NULL,
	job_id int4 NULL,
	industry_category_id int4 NULL,
	wealth_segment_id int4 NULL,
	property_valuation_id int4 NULL,
	CONSTRAINT customer_pkey PRIMARY KEY (customer_id),
	CONSTRAINT customer_address_id_fkey FOREIGN KEY (address_id) REFERENCES public.address(address_id),
	CONSTRAINT customer_gender_id_fkey FOREIGN KEY (gender_id) REFERENCES public.gender(gender_id),
	CONSTRAINT customer_industry_category_id_fkey FOREIGN KEY (industry_category_id) REFERENCES public.industry_category(industry_category_id),
	CONSTRAINT customer_job_id_fkey FOREIGN KEY (job_id) REFERENCES public.job(job_id),
	CONSTRAINT customer_property_valuation_id_fkey FOREIGN KEY (property_valuation_id) REFERENCES public.property_valuation(property_valuation_id),
	CONSTRAINT customer_wealth_segment_id_fkey FOREIGN KEY (wealth_segment_id) REFERENCES public.wealth_segment(wealth_segment_id)
);


-- public.product_price definition

-- Drop table

-- DROP TABLE public.product_price;

CREATE TABLE public.product_price (
	product_price_id int4 NOT NULL,
	product_id int4 NULL,
	list_price numeric(10, 2) NULL,
	standard_cost numeric(10, 2) NULL,
	price_date date NULL,
	CONSTRAINT product_price_pkey PRIMARY KEY (product_price_id),
	CONSTRAINT product_price_product_id_fkey FOREIGN KEY (product_id) REFERENCES public.product(product_id)
);


-- public.ref_product_order definition

-- Drop table

-- DROP TABLE public.ref_product_order;

CREATE TABLE public.ref_product_order (
	order_id int4 NULL,
	product_id int4 NULL,
	CONSTRAINT ref_product_order_order_id_fkey FOREIGN KEY (order_id) REFERENCES public."order"(order_id),
	CONSTRAINT ref_product_order_product_id_fkey FOREIGN KEY (product_id) REFERENCES public.product(product_id)
);


-- public."transaction" definition

-- Drop table

-- DROP TABLE public."transaction";

CREATE TABLE public."transaction" (
	transaction_id int4 NOT NULL,
	product_id int4 NULL,
	customer_id int4 NULL,
	order_id int4 NULL,
	transaction_date date NULL,
	CONSTRAINT transaction_pkey PRIMARY KEY (transaction_id),
	CONSTRAINT transaction_customer_id_fkey FOREIGN KEY (customer_id) REFERENCES public.customer(customer_id),
	CONSTRAINT transaction_order_id_fkey FOREIGN KEY (order_id) REFERENCES public."order"(order_id),
	CONSTRAINT transaction_product_id_fkey FOREIGN KEY (product_id) REFERENCES public.product(product_id)
);
```
### 5. Загрузить данные в таблицы в соответствии с созданной структурой (использовать команду INSERT INTO или загрузить файлы, используя возможности инструмента DBeaver; в случае загрузки файлами приложить скрины, что данные действительно были залиты).

Не успел.

