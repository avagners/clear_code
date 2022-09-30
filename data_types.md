1.
```python
if count_doc > 0:
    average_sum = sum_all_doc / count_doc
else:
    average_sum = 0

// перед делением проверил, что count_doc больше нуля
```

2.
```python
было:
int(5 / 2)

стало:
5 // 2

// заменил преобразование типа на операцию целочисленного деления
```

3.
```python
# файл с новыми точками доставки для загрузки
DP_FILE_NAME = 'new_dp_client.csv'

// вынес наименование файла с новыми точками доставки в константу, так как в коде встречался более 1 раза в даге airflow
```

4.
```python
# спец-код точки доставки для бонусных документов
BONUS_DP_CODE = 'free_product'

// вынес код точки доставки для бонусных документов в константу
```

5.
```python
...
ascending = 1 if self.__ascending else -1
compare = self.compare(val, node.value)
if compare != ascending:
    break
...
// создал логические переменные
```

6.
```python
# цвет фона
BACKGROUND_COLOR = "#B1DDC6"

// вынес цвет фона окна в константу
```

7.
```python
# загружаем df в базу
is_df_to_db = report.df_to_db(session_id)
if is_df_to_db:
    ...

// создал логическию переменную для повышения читаемости кода
```

8.
```python
# проверяем наличие файла с новыми точками для загрузки
exists_new_dp = os.path.exists(DP_FILE_NAME)
if not exists_new_dp:
    return False
...

// создал логическию переменную для повышения читаемости кода
```

9.
```python
...
# проверяем был ли изменен файл
is_modified_file = self.file_name == other.file_name and self.mdate != other.mdate

if is_modified_file:
    ...

// создал логическию переменную для повышения читаемости кода
```

10.
```python
# кол-во столбцов в df
COUNT_COLUMNS = 26

// вынес "магическое" число в константу
```

11.
```python
nw_df = df.select(
df.CALC_DT.cast("date").alias("calc_dt"),\
df.WHS_ID.cast("integer").alias("whs_id"),\
df.ART_ID.cast("integer").alias("art_id"),\
df.ART_GRP_LVL_3_ID.cast("integer").alias("art_grp_lvl_3_id"),\
df.ALGORITHM_TYPE.cast("integer").alias("algorithm_type"),\
df.FRST_BEGIN_DT.cast("date").alias("frst_begin_dt"),\
df.FRST_PRICE.cast("float").alias("frst_price"),\
df.SCND_PRICE.cast("float").alias("scnd_price"),\
df.FRST_TXN_CNT.cast("integer").alias("frst_txn_cnt"),\
df.SCND_TXN_CNT.cast("integer").alias("scnd_txn_cnt"),\
df.FRST_CLIENT_CNT.cast("integer").alias("frst_client_cnt"),\
df.SCND_CLIENT_CNT.cast("integer").alias("scnd_client_cnt"),\
df.FRST_DAY_CNT_REST_SALES.cast("integer").alias("frst_day_cnt_rest_sales"),\
df.SCND_DAY_CNT_REST_SALES.cast("integer").alias("scnd_day_cnt_rest_sales"),\
df.FRST_HOLIDAY_ID.cast("integer").alias("frst_holiday_id"),\
df.SCND_HOLIDAY_ID.cast("integer").alias("scnd_holiday_id"),\
df.FRST_REASON_TYPE_ID.cast("integer").alias("frst_reason_type_id"),\
df.SCND_REASON_TYPE_ID.cast("integer").alias("scnd_reason_type_id"),\
df.SSN_COEF_SALE_TXN.cast("float").alias("ssn_coef_sale_txn"),\
)

// преобразование float на integer, где это возможно
```

12.
```python
DISCOUNT_COEF = 0.8

price = float(data_sku_var[0]["price"]) * DISCOUNT_COEF

// вынес "магическое" число в константу
```
