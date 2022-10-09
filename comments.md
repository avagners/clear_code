3.1 

1. 
```python
// добавил комментарий к блоку кода в функции

...
@task
def check_emails(settings_subjects, email_list, **context):
    date = context['ti'].xcom_pull(task_ids='get_date')
    with ImapHook(imap_conn_id=CONFIG['imap_conn_name']) as imap:
        imap = imap.mail_client
        imap.select('INBOX')
        data = imap.search(None, f"ON {date}")[1]
        ids = data[0]
        mail_ids = ids.split()

        for id in mail_ids:
            data = imap.fetch(id, "(RFC822)")[1][0][1]
            email_message = email.message_from_bytes(data)
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

            if check_subject(settings_subjects, decoded_str):
                logger.info(f'{id} - Письмо не идентифицировано')
                send_email(email_list, attach=email_message)
                logger.info(f'{id} - Письмо отправленно ответственным лицам: {email_list}')
...         
```

2. 
```python
// добавил комментарий к списочному выражению

def validate_files(follow=False):
    """
    Проверяет все файлы в каталоге settings.INBOX_PATH
    :param follow: Bool, False - проверить файлы и завершить программу.
                         True - проверить файлы и ожидать новых.
    :return: None
    """
    logging.info('=' * 30)
    logging.info('Начало групповой валидации.')
    shared_data = {}
    try:
        while True:
            files = glob.glob(
                os.path.join(settings.INBOX_PATH, '*.xlsx'))
            # исключаем из проверки временные файлы excel
            files = [file for file in files if not file.strip().startswith('~')]
            for file in files:
                validate(file, shared_data)
            if not follow:
                break
            time.sleep(1)
    except KeyboardInterrupt:
        logging.info('Пользователь прервал выполение')
    logging.info('Завершение групповой валидации')
```

3. 
```python
// добавлен комментарий объясняющий причину увеличения
// индекса датафрейма на 2

...
df_promo = load_promo_file(promo_file_path_temp, temp_dir)
# добавим номер строки, 1 добавим поскольку индекс начинается с 0,
# а нам нужно с 1. И еще 1 - смещение из-за того что в оригинальном
# файле есть заголовок в первой строке теперь row_number соответствует
# номеру строки в оригинальном файле c отчетом
df_promo['row_number'] = df_promo.index + 2
...
```

4. 
```python
// добавлен комментарий, объясняющий наличие строки кода

...
# некоторые названия столбцов имеют лишние пробелы
df_promo.rename(columns=lambda x: x.strip(), inplace=True)
...
```

5. 
```python
// добавлен комментарий, объясняющий наличие строки кода

...
# переводим в подходящий формат скидки для дальнейшего сравнения
df_promo['action_disc'] = df_promo['action_disc'].apply(self.__convert_discont)
...
```

6. 
```python
// добавлен комментарий объясняющий преобразования датафрейма

...
def _files_to_df(self, shared_data):
    super()._files_to_df(shared_data)
    df_all = pd.concat(self.df_list)
    # удаляем повторяющиеся строки, оставляя только последние по дате
    # файлы отсортированы в обратном порядке, поэтому keep='first'
    df_all = df_all[~df_all.index.duplicated(keep='first')]
    return df_all
...
```

7. 
```python
// добавлен комментарий, объясняющий причину замены дат в локальных файлах

...
def get_local_files(self):
    local_files = glob.glob(os.path.join(self.local_promo_path, '*.csv'))
    remote_files = self.shared_data['19_sftp_promo']
    all_local_files = []
    for path in local_files:
        if os.path.getsize(path):
            new_file = File(path)
            # перекидываем данные о дате-времени изменения из 
            # удаленных файлов в локальную версию,
            # так как локальные файлы содержат даты создания на
            # локальной машине, а не изначальных файлов
            new_file.set_date(remote_files.get_date(new_file), date_str=False)
            all_local_files.append(new_file)
    all_local_files.sort(reverse=True)
    return all_local_files
...
```

3.2 

1. 
```python
// удален избыточный комментарий

...
# получаем список файлов *.xlsx в каталоге входящих
files = glob.glob(os.path.join(settings.INBOX_PATH, '*.xlsx'))
...
```

2. 
```python
// удален избыточный комментарий

...
# берем все строки датафрейма, кроме последней
df = df[:-1]
...
```

3. 
```python
// удален избыточный комментарий

...
# получаем список файлов *.zip в каталоге EECUSTOMER_PATH
files = glob.glob(os.path.join(self.settings.EECUSTOMER_PATH, '*.zip'))
...
```

4. 
```python
// удален избыточный комментарий

...
# подключаемся к FTP
with FTPHook(ftp_conn_id='ftp_conn_client') as ftp:
    ftp.get_conn().encoding = 'utf-8'
    ftp.test_connection()
    ...
```

5. 
```python
// удален избыточный комментарий

...
# получаем все xml документы в директории
files_dir = glob.glob(f'{path}/xml/{date_today}/**', recursive=True)
...
```
