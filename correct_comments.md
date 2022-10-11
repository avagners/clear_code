1. 
```python
// информативные комментарии

...
# поиск даты по формату '%b-%Y'
period = re.findall(r"^\w\w\w\D\d\d\d\d", self.df.iloc[0, 4])
...
```

2. 
```python
// информативные комментарии

...
# заменяем "май" на "мая"
if period[0].split('-')[0] == 'май':
    period[0] = 'мая-' + period[0].split('-')[1]
...
```

3. 
```python
// информативные комментарии

...
# фильтр по теме соответствующей клиенту
cust_theme = self.settings.get_cust_theme()
if cust_theme:
    t = '(SUBJECT "' + cust_theme + '")'
    criteria.append(t.encode('utf8'))
...
```

4. 
```python
// представление намерений

...
# фильтруемся по message_id из session_df, чтобы найти нужное email
df_this = self.session_df[lambda x: x['message_id'] == message_id]
...
```

5. 
```python
// представление намерений

...
# нужно проверить есть ли среди них успешно загруженные
df_success = df_this[df_this['successful'] == 1]
# если успешно загруженных нет или есть файл, которые не загружен,
# тогда нужно его загрузить
if df_success.empty or len(df_this) > len(df_success):
    msg2 = email.message_from_bytes(data, policy=policy.default)
    email_message = EmailMessage(
        message=msg2, sessions=df_this, received=internal_date
    )
...
```

6. 
```python
// информативные комментарии

...
# создаем zip-архив
file_name = path_file.split('/')[-1]
zip_name = f'{path_archive}/{self.session_id}.zip'
with ZipFile(zip_name, mode='w') as zf:
    zf.write(path_file, file_name)
    test_zip = zf.testzip()
    logger.info(
        f'Создан zip-архив: {zip_name}' if test_zip is None
        else f'Ошибка создания zip-архива: {test_zip}'
    )
...
```

7. 
```python
// информативные комментарии

...
# удаляем исходный файл
if os.path.exists(path_file):
    os.remove(path_file)
    logger.info(f'Удален файл из временной директории: {path_file}')
else:
    logger.info("Файл не найден.")
...
```

8. 
```python
// усиление

...
def df_to_db(self, df: pd.DataFrame, schema: str, table_name: str,
             session_id: int, index=False, index_label=None):
    # важно добавить номер сессии в датафрейм перед загрузкой в базу
    df.insert(0, 'session_id', session_id)
    df.to_sql(schema=schema, name=table_name, con=self.get_engine(),
              index=index, index_label=index_label,
              if_exists='append', method='multi', chunksize=100)
...
```

9. 
```python
// информативные комментарии

...
# параметры sql запроса передаем только в кортеже
params = tuple(params_in.values())
...
```

10. 
```python
// комментарии TODO

...
# TODO: сделать универсальную проверку
json_acceptable_string = object_text.replace("/Партнер", "")
...
```

11. 
```python
// комментарии TODO

# TODO: сделать получение параметра из xcom
def from_xcom(param):
    return 12354
```

12. 
```python 
// представление намерений

...
# отправляем отчёт по почте
send_email = EmailOperator(
    task_id='send_email',
    ...
```
