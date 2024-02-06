# Snowpark 101

<img src="./images/SnowparkIcon.png" alt="Snowpark Flow Overview" style="width:30%;display:block;margin:0 auto;" />


# Contenido
- Mostrar como usar Snowflake Snowpark con Python

## 1 Script para crear objetos Snowflake antes de ejecutar los Notebooks

```sql

use role accountadmin;

create or replace  database SP2023;

create or replace warehouse python_wh
with warehouse_size = 'XSMALL'
warehouse_type = 'STANDARD'
auto_suspend = 60
auto_resume = TRUE;


-- Actviar Objetos Snowflake 
use database SP2023;
use warehouse python_wh;
use schema public;

-- Objeto Tabla 
CREATE or replace TABLE ORDERS_TBL (ISFRAUD String,
    TRNX_ID String,
    IP_ADDRESS String,
    CITY String,
    SHIPPING_ZIPCODE String,
    SHIPPING_STATE String,
    PAYMENT_NETWORK String,
    PAYMENT_TYPE String,
    TOTAL_TRNX_AMOUNT String,
    JOIN_KEY String);

--roles
create role devrole;

-- privilegios a roles 
grant all privileges on database SP2023 to role devrole;
grant all privileges on schema SP2023.public to role devrole;
grant usage on database SP2023 to role devrole;
grant select on table SP2023.PUBLIC.ORDERS_TBL to role devrole;
grant usage on schema SP2023.PUBLIC  to role devrole;
GRANT ALL ON ALL TABLES IN SCHEMA SP2023.PUBLIC TO ROLE devrole;
grant select on future tables in database SP2023 to role devrole;
grant usage on future schemas in database SP2023 to role devrole;
grant usage on warehouse python_wh to role devrole;


```


## 2 Creación de ambiente de desarrollo
- Ya instalado Anaconda, Python (3.10), se crea el ambiente de desarrollo 
```bash

conda env create -f ambiente.yml

```

## 2.1 Activación de ambiente de desarrollo

```bash

conda activate  snowpark101

```

## 3 Uso de Notebook conexion.ipynb
- En este script se muestran los pasos para cargar archivo CSVy uso de Snowpark framework


## 4 Validación en Snowflake de carga de datos
- Script para validar data en los objetos

```sql

-- Seleción de datos

use role devrole;

select * from ORDERS_TBL limit 10;

-- select * from SP2023.PUBLIC.TOP_5_TBL;

```

## 5 Limpieza
- Script para limpieza de objetos

```sql

-- admin clean
use role accountadmin;
drop table orders_TBL;

--devrole  clean
use role devrole;
--truncate table ORDERS_TBL;
drop stage orders_stage;
drop table TOP_5_TBL;
drop file format CSV;
drop function SP2023.PUBLIC.CALCULATE_CIRCUMFERENCE_TEMP(FLOAT);

```
