1. 
```python
// список заменил на очередь

from queue import Queue
...
def get_files(self) -> Queue:
    files = Queue()
    for part in self.message.walk():
        content = EmailMessageContent(part)
        if content.is_file:
            df_this = self.sessions[(
                self.sessions['file_name'] == content.file_name
            ) & (self.sessions['successful'] == True)]
            if df_this.empty:
                files.put(content)
        else:
            del content
    return files
...
```

2.  
```python
// список заменил на множество

...
def get_mails_for_load(self) -> Queue:
    criteria = self._get_criteria()

    _, value = self.imap.search('utf-8', *criteria)
    messages = [x for x in value[0].split(b' ') if x != b'']

    reports = set()
    for msg in messages:
        ...
        for email_file in email_message:
            report = MailReport(
                file_settings_id=self.file_settings_id.get_byte_array(),
                message_id=message_id,
                date_recieved=email_message.received_date,
                emailcontent=email_file,
                cust_id=self.cust_id
            )
            reports.add(report)
    return reports
...
```

3.  
```python
// список заменил на очередь

from queue import Queue
...
def read_zip(self) -> list:
    fileobj = io.BytesIO(self.part.get_content())
    extension = {'.xls': 'xlrd', '.xlsx': 'openpyxl', '.xlsb': 'pyxlsb'}

    zip_archive = ZipFile(fileobj)
    zip_files = zip_archive.namelist()

    queue_dfs = Queue()
    for file in zip_files:
        ext = os.path.splitext(file)[1]
        if ext in extension.keys():
            df = pd.read_excel(
                zip_archive.read(file),
                engine=extension[ext],
                header=None
            )
            df.dropna(axis=1, how='all', inplace=True)
            df.fillna(0, inplace=True)
            queue_dfs.put(df)
    return queue_dfs
...
```

4. 
```python
// список заменил на множество

def get_email_list():
    sql = """
        select
            ml.employee_email
        from pos.mailing_lists ml
        where
            ml.list_code = 0xB824005056A1200211ED0C1169A12083
        """
    mssql = MsSqlHook(mssql_conn_id=CONFIG['db_conn_name'])
    connection = mssql.get_conn()
    cursor = connection.cursor()
    cursor.execute(sql)

    emails_set = set()
    for row in cursor:
        emails_set.add(row[0])

    connection.commit()
    cursor.close()
    logger.info(f'Получили список ответсвтенных: {emails_set}')
    return emails_set
```

5. 
```python
// список заменил на множество

def get_email_subjects_from_db():
    sql = """
        select distinct
            fs.report_settings_value_string
        from pos.file_settings fs
        where
            fs.report_settings_name = 'Тема письма'
        """

    mssql = MsSqlHook(mssql_conn_id=CONFIG['db_conn_name'])
    connection = mssql.get_conn()
    cursor = connection.cursor()
    cursor.execute(sql)
    subjects = set()
    for row in cursor:
        subjects.add(row[0].lower())
    connection.commit()
    cursor.close()
    return subjects
```
