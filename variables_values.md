1. 
```python
// Создание переменной "sql" перед первым обращением к ней

До:
def convert_to_excel(session_id, path, date_today):
    sql = f"""
        select
            product_ean,
            product_scu,
            product_name,
            product_cs,
            qty_rest,
            sales
        from dbo.rest
        where session_id = {session_id}
        """
    mssql = MsSqlHook(mssql_conn_id='mssql_datalake')
    engine = create_engine('mssql+pymssql://', creator=mssql.get_conn)
    df = pd.read_sql(sql, engine)
    name_file = f'{path}/rest/rest_{date_today}.xlsx'
    df.to_excel(name_file, index=False)
    return name_file

После:
def convert_to_excel(session_id, path, date_today):
    mssql = MsSqlHook(mssql_conn_id='mssql_datalake')
    engine = create_engine('mssql+pymssql://', creator=mssql.get_conn)
    sql = f"""
        select
            product_ean,
            product_scu,
            product_name,
            product_cs,
            qty_rest,
            sales
        from dbo.rest
        where session_id = {session_id}
        """
    df = pd.read_sql(sql, engine)
    name_file = f'{path}/rest/rest_{date_today}.xlsx'
    df.to_excel(name_file, index=False)
    return name_file
```

2. 
```python
// Создание переменной "sql" перед первым обращением к ней

До:
def sales_import(session_id):
    '''
    Выгрузка данных по продажам из базы и сохранение
    в таблицу
    '''
    sql = """
          exec dbo.sales_import @session_id = %s;
          """
    mssql = MsSqlHook(mssql_conn_id='mssql_datalake')
    connection = mssql.get_conn()
    cursor = connection.cursor()
    parameters = (ses_id)
    cursor.execute(sql, parameters)
    connection.commit()
    cursor.close()
    return ses_id

После:
def sales_import(session_id):
    '''
    Выгрузка данных по продажам из базы и сохранение
    в таблицу
    '''
    mssql = MsSqlHook(mssql_conn_id='mssql_datalake')
    connection = mssql.get_conn()
    cursor = connection.cursor()
    sql = """
          exec dbo.sales_import @session_id = %s;
          """
    parameters = (session_id)
    cursor.execute(sql, parameters)
    connection.commit()
    cursor.close()
    return session_id
```

3. 
```python
// Вывод предупреждения в случае обнаружения ошибок в отчёте

...
if count_error > 0:
    logging.warning(f'Валидатор {validator} вернул {count_error} строк с ошибками')
else:
    logging.info('Валидатор {validator} ошибок не обнаружил.')
...
```

4. 
```python
// Вывод предупреждения в случае обнаружения ошибок в отчёте

...
if field not in df.columns:
    logging.error(f'Валидатор вернул некорректные данные.'
                   'В данных отсутствует поле "{field}".')
...
```

5. 
```python
// Удалил неиспользуемую переменную

Было:
def error_to_excel(promo_file_path, errors_count, promo_file_path_temp, df_errors, df_promo):
    ...

Стало:
def error_to_excel(promo_file_path, errors_count, promo_file_path_temp, df_errors):
    ...
```

6. 
```python
// Присвоение "недопустимого" значения переменной после её использования

До:
errors_count = 0
for validator in validators:
    ...

После:
errors_count = 0
for validator in validators:
    ...
errors_count = -1
```

7. 
```python
// Присвоение "недопустимого" значения переменной после её использования

До:
error_count = len(df_validator_errors[df_validator_errors['errors'] != ''].index)
...

После:
error_count = len(df_validator_errors[df_validator_errors['errors'] != ''].index)
...
error_count = -1
```

8. 
```python 
// Создание переменной непосредственно перед циклом

До:
...
all_selected_files = []
...
for file in new_files_list.selected_files:
    all_selected_files.append(file)
...

После:
...
all_selected_files = []
for file in new_files_list.selected_files:
    all_selected_files.append(file)
...
```

9. 
```python
// Удалил неиспользуемую переменную

До:
def check_promo(self, x):
    check_list = ['action_type', 'action_disc']
    ...

После:
def check_promo(self, x):
    ...
```

10. 
```python
// Вывод предупреждения в случае обнаружения ошибок в отчёте
...
if x['date_price'] > x['end_date']:
    logging.error("Дата формирования цены позже окончания промо.")
...
```

11. 
```python
// Удалил неиспользуемую переменную "name" в цикле

До:
...
if len(self.files) == len(new_files):
    for index, name in enumerate(self.files):
        ...

После:
...
if len(self.files) == len(new_files):
    for index range(len(new_files)):
        ...
```

12. 
```python
// Присвоение "недопустимого" значения переменной после её использования

До:
...
unique_sku_count = 0
sku_list_from_df = mini_df['sku_code']
for sku in sku_list_from_df:
    if sku in self.sku_codes:
        unique_sku_count += 1
    ...

После:
...
unique_sku_count = 0
sku_list_from_df = mini_df['sku_code']
for sku in sku_list_from_df:
    if sku in self.sku_codes:
        unique_sku_count += 1
    ...
unique_sku_count = -1
...
```

13. 
```python
// Создание переменной непосредственно перед циклом

До:
...
error3 = ''
...
for must_sku_item in must_sku_list:
    if must_sku_item not in sku_list_checked:
        error3 = 'Отсутствует обязательный CSKU для покупки. '
...

После:
...
error3 = ''
for must_sku_item in must_sku_list:
    if must_sku_item not in sku_list_checked:
        error3 = 'Отсутствует обязательный CSKU для покупки. '
...
```

14. 
```python
// Создание переменной непосредственно перед циклом. Cоздание логических переменных

До:
...
errors = ''
...
for field in self.settings.PROMO_FILE_COLUMNS.keys():
    if field not in self.settings.PROMO_EXTRA_SPACES_VALUES and str(x[field]) != str(x[field]).strip():
        errors += f'Поле "{field}" содержит лишние пробелы. '
...

После:
...
errors = ''
for field in self.settings.PROMO_FILE_COLUMNS.keys():
    is_not_exist_field = field not in self.settings.PROMO_EXTRA_SPACES_VALUES
    is_extra_spaces = str(x[field]) != str(x[field]).strip()
    if is_not_exist_field and is_extra_spaces:
        errors += f'Поле "{field}" содержит лишние пробелы. '
...
```

15. 
```python
// Присвоение "недопустимого" значения переменной после её использования

До:
...
current_file = MDFile(path)
if current_file.prefix == self.prefix:
    current_file.set_date()
    prefix_files.append(current_file)
...

После:
...
current_file = MDFile(path)
if current_file.prefix == self.prefix:
    current_file.set_date()
    prefix_files.append(current_file)
current_file = None
...
```