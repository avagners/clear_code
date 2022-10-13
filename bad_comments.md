1. 
```python
// удалил закомментированный код

Было:
def load_promo_file(promo_file_path, temp_dir):
    """Возвращает датафрейм с данными промо-отчета"""
    # usecols = [x[0] for x in enumerate(settings.PROMO_FILE_COLUMNS) if x[1] in settings.PROMO_FILE_COLUMNS_TO_LOAD]
    # colnames = list(map(settings.PROMO_FILE_COLUMNS.__getitem__, usecols))
    # date_columns = [x[0] for x in enumerate(settings.PROMO_FILE_COLUMNS_TO_LOAD) if x[1] in settings.PROMO_FILE_DATE_COLUMNS]
    # dtype = {x[0]: settings.PROMO_FILE_DTYPES[x[1]] for x in enumerate(settings.PROMO_FILE_COLUMNS_TO_LOAD) if x[1] in settings.PROMO_FILE_DTYPES.keys()}
    usecols = np.arange(31)
    colnames = list(settings.PROMO_FILE_COLUMNS.keys())
    ...

Стало:
def load_promo_file(promo_file_path, temp_dir):
    """Возвращает датафрейм с данными промо-отчета"""
    PROMO_FILE_DTYPES.keys()}
    usecols = np.arange(31)
    colnames = list(settings.PROMO_FILE_COLUMNS.keys())
    ...
```

2. 
```python
// удалил закомментированный код

...
# def update(self, shared_data):
#     need_update = self.check_update()
#     if need_update == 'need_update':
#         self.update_files(shared_data)
#         self.update_time = datetime.now()
#         return True
#     elif need_update == 'need_update_file':
#         self.get_files_attr()
#     else:
#         return False
...
```

3. 
```python
// удалил закомментированный код

Было:
def _get_df(self, file):
    df = pd.read_csv(file.full_path, encoding='cp1251', sep=';',
                     usecols=self.settings.RD_BASIC_COL.keys(),
                     dtype=self.settings.RD_BASIC_COL)
    df.rename(columns=self.settings.RD_BASIC_FILE_COLUMNS_CONVERTER, inplace=True)
    #df['file_name'] = file.file_name
    return df

Стало:
 def _get_df(self, file):
    df = pd.read_csv(file.full_path, encoding='cp1251', sep=';',
                     usecols=self.settings.RD_BASIC_COL.keys(),
                     dtype=self.settings.RD_BASIC_COL)
    df.rename(columns=self.settings.RD_BASIC_FILE_COLUMNS_CONVERTER, inplace=True)
    return df   
```

4. 
```python
// Удалил шум. Утверждение очевидного.

Было:
# конечная таблица для проверки
end = pd.merge(
    diff_type_df, df_promo_copy, on=[
    'doc_number', 'sku_code', 'avg_promo_disc', 'date_shipment', 'doc_quantity'
    ], 
    how='inner'
)

Стало:
end_df = pd.merge(
    diff_type_df, df_promo_copy, on=[
    'doc_number', 'sku_code', 'avg_promo_disc', 'date_shipment', 'doc_quantity'
    ], 
    how='inner'
)
```

5. 
```python
// Удалил закомментированный код и неочевидные комментарии

Было:
def check_promo(self, x):
    check_list = ['action_type', 'action_disc']  # 'gold', 'store_type',
    errors = ''
    # if x['pid'] == '-' or 'LOCAL' in x['sid']:
    # return False
    # else:
    ...

Стало:
def check_promo(self, x):
    check_list = ['action_type', 'action_disc']
    errors = ''
    ...
```

6. 
```python
// Удалил закомментированный код

...
if x['date_price'] > x['end_date']:  # or x['date_price'] < x['start_date']
    errors += "Дата формирования цены позже окончания промо. "
...
```

7. 
```python
// Не используйте комментарии там, где можно использовать функцию или переменную.
// Одну функцию разбил на 2.

Было:
@task
def check_emails(settings_subjects, email_list, **context):
    date = context['ti'].xcom_pull(task_ids='get_date')
    with ImapHook(imap_conn_id=CONFIG['imap_conn_name']) as imap:
        ...
        mail_ids = ids.split()
        for id in mail_ids:
            ...
            header_subject = email.header.decode_header(
                email_message['Subject']
            )
            # декодируем тему письма
            decoded_str = ''
            for part, encoding in header_subject:
                if type(part) is not str and encoding:
                    email_subject = part.decode(encoding).lower()
                else:
                    email_subject = part.lower()
                if type(email_subject) == str:
                    decoded_str += email_subject
            ...
            if check_subject(settings_subjects, decoded_str):
                ...

Стало:
def decoder_header_subject(header_subject) -> str:
    decoded_str = ''
    for part, encoding in header_subject:
        if type(part) is not str and encoding:
            email_subject = part.decode(encoding).lower()
        else:
            email_subject = part.lower()
        if type(email_subject) == str:
            decoded_str += email_subject
    return decoded_str

@task
def check_emails(settings_subjects, email_list, **context):
    date = context['ti'].xcom_pull(task_ids='get_date')
    with ImapHook(imap_conn_id=CONFIG['imap_conn_name']) as imap:
        ...
        mail_ids = ids.split()
        for id in mail_ids:
            ...
            header_subject = email.header.decode_header(
                email_message['Subject']
            )
            decoded_str = decoder_header_subject(header_subject)
            ...
            if check_subject(settings_subjects, decoded_str):
                ...
```

8. 
```python
// Не используйте комментарии там, где можно использовать функцию или переменную.
// Один метод разбил на 2.

Было:
def call_proc(self, proc_name, params_in={}, params_out={}):
    connection = self.mssql.get_conn()
    cursor = connection.cursor()
    # генерация sql-запроса вызова хранимой процедуры
    output_param_type = list(params_out.values())
    output_param_name = list(params_out.keys())
    if output_param_type:
        output_param_beg = f'declare {output_param_name[0]} {output_param_type[0]};'
        output_param_end = f'select {output_param_name[0]} as the_output;'
    params = tuple(params_in.values())
    param_name = params_in.keys()
    params_name_str = '= %s, '.join(param_name)
    if params:
        params_name_str += " = %s"
    if output_param_type:
        if params:
            params_name_str += ","
        params_name_str += f'{output_param_name[0]} = {output_param_name[0]} output'
        sql = f'{output_param_beg}\nexec {proc_name} {params_name_str};\n{output_param_end}'
    else:
        sql = f'exec {proc_name} {params_name_str}'
    # вызов созданного запроса
    cursor.execute(sql, params)
    if output_param_type:
        rows = cursor.fetchone()
        connection.commit()
        cursor.close()
        return rows[0]
    else:
        connection.commit()
        cursor.close()
        return True

Стало:
def create_sql_query(self, proc_name, params_in={}, params_out={}):
    output_param_type = list(params_out.values())
    output_param_name = list(params_out.keys())
    if output_param_type:
        output_param_beg = f'declare {output_param_name[0]} {output_param_type[0]};'
        output_param_end = f'select {output_param_name[0]} as the_output;'
    # параметры sql запроса передаем только в кортеже
    params = tuple(params_in.values())
    param_name = params_in.keys()
    params_name_str = '= %s, '.join(param_name)
    if params:
        params_name_str += " = %s"
    if output_param_type:
        if params:
            params_name_str += ","
        params_name_str += f'{output_param_name[0]} = {output_param_name[0]} output'
        sql = f'{output_param_beg}\nexec {proc_name} {params_name_str};\n{output_param_end}'
    else:
        sql = f'exec {proc_name} {params_name_str}'
    return sql, params, output_param_type

def call_proc(self, proc_name, params_in={}, params_out={}):
    connection = self.mssql.get_conn()
    cursor = connection.cursor()
    sql, params, output_param_type = self.create_sql_query(proc_name, params_in, params_out)
    cursor.execute(sql, params)
    if output_param_type:
        rows = cursor.fetchone()
        connection.commit()
        cursor.close()
        return rows[0]
    else:
        connection.commit()
        cursor.close()
        return True
```

9. 
```python
// Не используйте комментарии там, где можно использовать функцию или переменную.
// Один метод разбил на 3.

Было:
def archive(self, path_file, path_archive):
    if not os.path.exists(path_archive):
        os.makedirs(path_archive)
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
    # удаляем исходный файл
    if os.path.exists(path_file):
        os.remove(path_file)
        logger.info(f'Удален файл из временной директории: {path_file}')
    else:
        logger.info("Файл не найден.")
    return True

Стало:
def _create_zip(self, path_file, path_archive):
    file_name = path_file.split('/')[-1]
    zip_name = f'{path_archive}/{self.session_id}.zip'
    with ZipFile(zip_name, mode='w') as zf:
        zf.write(path_file, file_name)
        test_zip = zf.testzip()
        logger.info(
            f'Создан zip-архив: {zip_name}' if test_zip is None
            else f'Ошибка создания zip-архива: {test_zip}'
        )
    return True

def _delete_file(self, path_file):
    if os.path.exists(path_file):
        os.remove(path_file)
        logger.info(f'Удален файл из временной директории: {path_file}')
    else:
        logger.info("Файл не найден.")

def archive(self, path_file, path_archive):
    if not os.path.exists(path_archive):
        os.makedirs(path_archive)
    self._create_zip(path_file, path_archive)
    self._delete_file(path_file)
    return True
```

10. 
```python
// удалил закомментированный код

Было:
...
for num in data[0].split():
    # subject = mail.fetch(num,'(BODY.PEEK[HEADER.FIELDS (SUBJECT)])')[1][0][1].strip()
    # log.info(subject)
    result, data = mail.fetch(num, '(RFC822)')
    raw_email = data[0][1]
    ...

Стало:
...
for num in data[0].split():
    result, data = mail.fetch(num, '(RFC822)')
    raw_email = data[0][1]
    ...
```

11. 
```python
// удалил закомментированный код

Было:
...
try:
    os.makedirs(workingdir, mode=0o777, exist_ok=True)
except Exception as e:
    # return e
    log.critical(e, exc_info=True)
...

Стало:
...
try:
    os.makedirs(workingdir, mode=0o777, exist_ok=True)
except Exception as e:
    log.critical(e, exc_info=True)
...
```

12. 
```python
// Удалил шум. Утверждение очевидного.

Было:
...
# загружаем файл в датафрейм
df_promo = load_promo_file(promo_file_path_temp, temp_dir)
...

Стало:
...
df_promo = load_promo_file(promo_file_path_temp, temp_dir)
...
```

13. 
```python
// Удалил позиционные маркеры

Было:
# ---------------------------- PASSWORD GENERATOR ------------------------------- #
# Password Generator Project
def generate_password():
    ...

# ---------------------------- SAVE PASSWORD ------------------------------- #
def save():
    ...

# ---------------------------- FIND PASSWORD ------------------------------- #
def find_password():
    ...

Стало:
def generate_password():
    ...

def save_password():
    ...

def find_password():
    ...
```

14. 
```python
// Удалил обязательные комментарии. Шум. Утверждение очевидного.

Было:
def save_csv(data_dict):
    """
    Функция сохраненяет итоговый файл в формате csv.
    """
    ...

Стало:
def save_csv(data_dict):
    ...
```

15. 
```python
// Удалил позиционные маркеры. Шум. Утверждение очевидного.

Было:
# --------------------------------------- 5. СОЗДАНИЕ СТОЛБЧАТОЙ ДИАГРАММЫ  ---------------------------------------- #
def create_diagram(dict_data):
    plt.title('Статистика жанров')
    plt.xlabel("Название жанра")
    plt.ylabel("Кол-во повторений")
    plt.subplots_adjust(bottom=0.3)
    ...

Стало:
def create_diagram(dict_data):
    plt.title('Статистика жанров')
    plt.xlabel("Название жанра")
    plt.ylabel("Кол-во повторений")
    plt.subplots_adjust(bottom=0.3)
    ...
```