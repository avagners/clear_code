1. 
```python
// создание приватных переменных в классе

class Settings:
    __field_name = 'report_settings_name'

    def __init__(self, engine, file_settings_id):
        self.file_settings_id = file_settings_id
        self.engine = engine
        self.__settings = self.__get_settings()
    ...
```

2. 
```python
// группировка связанных команд в приватный метод

...
def _get_criteria(self):
    criteria = []
    self.imap.select("INBOX")
    if self.min_date and type(self.min_date) != float:
        # фильтр по дате
        criteria.append(
            '(SINCE ' + self.min_date.strftime('%d-%b-%Y') + ')'
        )
    # фильтр по теме соответствующей клиенту
    cust_theme = self.settings.get_cust_theme()
    if cust_theme:
        t = '(SUBJECT "' + cust_theme + '")'
        criteria.append(t.encode('utf8'))
    return criteria

def get_mails_for_load(self):
    criteria = self._get_criteria()
    ...
```

3. 
```python
// создание приватного метода

...
def __get_settings_value(self, settings_name, is_code=True):
    if is_code:
        filter_field = 'report_settings_value_code'
    else:
        filter_field = 'report_settings_value_string'
    settings_field = self.__settings[self.__settings[self.field_name] == settings_name][filter_field].squeeze()
    if is_code:
        return HoustonGuid(settings_field)
    return settings_field
...
```

4. 
```python
// создание приватного метода

...
@staticmethod
def _from_hex(full_string) -> bytearray:
    return bytearray.fromhex(full_string.replace('0x', ''))
...
```

5. 
```python
// создание приватного метода

...
@staticmethod
def _decode_bytes(filename):
    filename, encoding = decode_header(filename)[0]
    if isinstance(filename, bytes):
        return filename.decode(encoding)
    return filename
...
```

6.

```python
// группировка связанных команд в метод

...
def read_zip(self) -> list:
    '''
    Метод получает все файлы формата excel из zip-архива
    и возвращает список df
    '''
    fileobj = io.BytesIO(self.part.get_content())
    extension = {'.xls': 'xlrd', '.xlsx': 'openpyxl', '.xlsb': 'pyxlsb'}

    zip_archive = ZipFile(fileobj)
    zip_files = zip_archive.namelist()

    list_df = []
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
            list_df.append(df)
    return list_df
...
```

7. 
```python
// группировка связанных команд в метод

...
def _internaldate2datetime(internaldate):
    str_datetime = internaldate.decode().split('"')[1]
    msg_internaldate = email.utils.parsedate_to_datetime(str_datetime)
    return msg_internaldate.astimezone()
...
```

8. 
```python
// создание приватного метода

...
def _get_df(self, file):
    df = pd.read_csv(file.full_path, encoding='cp1251', sep=';',
                     usecols=self.settings.RD_BASIC_COL.keys(),
                     dtype=self.settings.RD_BASIC_COL)
    df.rename(
        columns=self.settings.RD_BASIC_FILE_COLUMNS_CONVERTER,
        inplace=True
    )
    return df'
...
```

9. 
```python
// группировка связанных команд в приватный метод

...
def _get_file_list(self, shared_data):
    files = glob.glob(os.path.join(self.settings.RD_BASIC_PATH, '*.csv'))
    files.sort(reverse=True)
    selected_files = []
    for file in files:
        new_files_list = File(file)
        new_files_list.set_date()
        selected_files.append(new_files_list)
    return selected_files
...
```

10. 
```python
// группировка связанных команд в метод

...
def get_prefix_files(self, files_list):
    prefix_files = []
    for path in files_list:
        current_file = MDFile(path)
        if current_file.prefix == self.prefix:
            current_file.set_date()
            prefix_files.append(current_file)
    return prefix_files
...
```

11. 
```python
// группировка связанных команд в метод

...
def get_all_files_in_month(self, month_date):
    month_files = []
    for file in self.prefix_files:
        if file.eq_date(month_date):
            month_files.append(file)
    return month_files
...
```

12. 

```python
// группировка связанных команд в отдельный метод

...
def check_null(self):
    check_list = ['action_quantity', 'doc_quantity', 'action_type']
    for check_col in check_list:
        if self.df_promo[check_col].isnull().values.any():
            return False
    return True
...
```

13. 
```python
// группировка связанных команд в отдельный метод

...
def split_full_path(self):
    file_name_with_ext = os.path.basename(self.full_path)
    path = os.path.dirname(self.full_path)
    filemass = os.path.splitext(file_name_with_ext)
    file_name = filemass[0]
    file_ext = filemass[1][1:]
    return path, file_name, file_ext
...
```

14. 
```python
// группировка связанных команд в отдельный метод

...
def check_must_sku_list(self, sku_list_checked):
    must_sku_list = self.must_sku.split(':')
    error3 = ''
    for must_sku_item in must_sku_list:
        if must_sku_item not in sku_list_checked:
            error3 = 'Отсутствует обязательный CSKU для покупки. '
    return error3
...
```

15. 
```python
// создание приватной переменной в классе

class DPBuilder:
    def __init__(self, http_connection, cust_id):
        """
        :param http_connection: соединение http_hook
        :param cust_id: id клиента в формате строки для api
        """
        self.__root = DeliveryPointItem(http_connection, cust_id)
```
