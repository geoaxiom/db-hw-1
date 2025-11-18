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

<img width="1170" height="1418" alt="hw1" src="https://github.com/user-attachments/assets/4df0c17f-56c7-4156-b79f-f714df17472d" />

### 3. Нормализовать базу данных (от 1НФ до 3НФ), описав, к какой нормальной форме приводится таблица и почему таблица в этой нормальной форме изначально не находилась.
Плоские таблици витрин `tmp_transaction` и `tmp_customer` изначально находились в 1NF

### 4. Создать все таблицы в DBeaver, указав первичные ключи к таблицам, правильные типы данных, могут ли поля быть пустыми или нет (использовать команду CREATE TABLE).
```
CREATE TABLE country (
	country_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	country_name varchar(50) NULL,
	CONSTRAINT country_pkey PRIMARY KEY (country_id)
);

CREATE TABLE gender (
	gender_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	gender_name varchar(255) NULL,
	CONSTRAINT gender_pkey PRIMARY KEY (gender_id)
);

CREATE TABLE industry_category (
	industry_category_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	industry_category_name varchar(255) NULL,
	CONSTRAINT industry_category_pkey PRIMARY KEY (industry_category_id)
);

CREATE TABLE job (
	job_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	job_title varchar(255) NULL,
	CONSTRAINT job_pkey PRIMARY KEY (job_id)
);

CREATE TABLE "order" (
	order_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	online_order bool NULL,
	order_status varchar(50) NULL,
	CONSTRAINT order_pkey PRIMARY KEY (order_id)
);

CREATE TABLE product (
	product_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	product_line varchar(50) NULL,
	product_class varchar(50) NULL,
	brand varchar(100) NULL,
	product_size varchar(50) NULL,
	CONSTRAINT product_pkey PRIMARY KEY (product_id)
);

CREATE TABLE property_valuation (
	property_valuation_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	property_valuation_value int4 NULL,
	CONSTRAINT property_valuation_pkey PRIMARY KEY (property_valuation_id)
);

CREATE TABLE state (
	state_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	state_name varchar(50) NULL,
	CONSTRAINT state_pkey PRIMARY KEY (state_id)
);

CREATE TABLE tmp_customer (
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

CREATE TABLE tmp_transaction (
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

CREATE TABLE wealth_segment (
	wealth_segment_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	wealth_segment_name varchar(255) NULL,
	CONSTRAINT wealth_segment_pkey PRIMARY KEY (wealth_segment_id)
);

CREATE TABLE address (
	address_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	address varchar(255) NULL,
	postcode varchar(20) NULL,
	state_id int4 NULL,
	country_id int4 NULL,
	CONSTRAINT address_pkey PRIMARY KEY (address_id),
	CONSTRAINT address_country_id_fkey FOREIGN KEY (country_id) REFERENCES country(country_id),
	CONSTRAINT address_state_id_fkey FOREIGN KEY (state_id) REFERENCES state(state_id)
);

CREATE TABLE customer (
	customer_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
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
	CONSTRAINT customer_address_id_fkey FOREIGN KEY (address_id) REFERENCES address(address_id),
	CONSTRAINT customer_gender_id_fkey FOREIGN KEY (gender_id) REFERENCES gender(gender_id),
	CONSTRAINT customer_industry_category_id_fkey FOREIGN KEY (industry_category_id) REFERENCES industry_category(industry_category_id),
	CONSTRAINT customer_job_id_fkey FOREIGN KEY (job_id) REFERENCES job(job_id),
	CONSTRAINT customer_property_valuation_id_fkey FOREIGN KEY (property_valuation_id) REFERENCES property_valuation(property_valuation_id),
	CONSTRAINT customer_wealth_segment_id_fkey FOREIGN KEY (wealth_segment_id) REFERENCES wealth_segment(wealth_segment_id)
);

CREATE TABLE product_price (
	product_price_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	product_id int4 NULL,
	list_price numeric(10, 2) NULL,
	standard_cost numeric(10, 2) NULL,
	price_date date NULL,
	CONSTRAINT product_price_pkey PRIMARY KEY (product_price_id),
	CONSTRAINT product_price_product_id_fkey FOREIGN KEY (product_id) REFERENCES product(product_id)
);

CREATE TABLE ref_product_order (
	order_id int4 NULL,
	product_id int4 NULL,
	CONSTRAINT ref_product_order_order_id_fkey FOREIGN KEY (order_id) REFERENCES "order"(order_id),
	CONSTRAINT ref_product_order_product_id_fkey FOREIGN KEY (product_id) REFERENCES product(product_id)
);

CREATE TABLE "transaction" (
	transaction_id int4 GENERATED ALWAYS AS IDENTITY( INCREMENT BY 1 MINVALUE 1 MAXVALUE 2147483647 START 1 CACHE 1 NO CYCLE) NOT NULL,
	product_id int4 NULL,
	customer_id int4 NULL,
	order_id int4 NULL,
	transaction_date date NULL,
	CONSTRAINT transaction_pkey PRIMARY KEY (transaction_id),
	CONSTRAINT transaction_customer_id_fkey FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
	CONSTRAINT transaction_order_id_fkey FOREIGN KEY (order_id) REFERENCES "order"(order_id),
	CONSTRAINT transaction_product_id_fkey FOREIGN KEY (product_id) REFERENCES product(product_id)
);
```
### 5. Загрузить данные в таблицы в соответствии с созданной структурой (использовать команду INSERT INTO или загрузить файлы, используя возможности инструмента DBeaver; в случае загрузки файлами приложить скрины, что данные действительно были залиты).

Не успел загрузить скриншоты до дедлайна.

<img width="538" height="264" alt="Screenshot from 2025-11-18 23-23-27" src="https://github.com/user-attachments/assets/592cb736-67a6-46a2-a0ab-67c33f911db7" />

<img width="653" height="269" alt="Screenshot from 2025-11-18 23-32-12" src="https://github.com/user-attachments/assets/744fb18d-50b5-4bac-8cbf-9f30f8d23b92" />

<img width="677" height="358" alt="Screenshot from 2025-11-18 23-36-31" src="https://github.com/user-attachments/assets/a76a1bce-c083-4194-96f1-563742ba2ef9" />

<img width="662" height="432" alt="Screenshot from 2025-11-18 23-42-20" src="https://github.com/user-attachments/assets/b914b2ae-5819-4bbb-b09a-ce35e424e078" />

<img width="778" height="434" alt="Screenshot from 2025-11-18 23-47-38" src="https://github.com/user-attachments/assets/e0ffd171-930c-462a-969b-708d35050d7f" />

<img width="736" height="377" alt="Screenshot from 2025-11-18 23-52-50" src="https://github.com/user-attachments/assets/1f025383-8d9b-49b3-a800-f81ffc132c91" />

<img width="907" height="665" alt="Screenshot from 2025-11-19 00-03-30" src="https://github.com/user-attachments/assets/35fb9e6d-fa25-4b2c-bfbd-826f1c2f3aa1" />

<img width="963" height="471" alt="Screenshot from 2025-11-19 00-15-01" src="https://github.com/user-attachments/assets/b03f0b81-852b-47eb-bee1-b19f387b2d87" />

<img width="1000" height="609" alt="Screenshot from 2025-11-19 00-41-01" src="https://github.com/user-attachments/assets/79a359cb-4808-4a6c-be79-46b3410c76d2" />


