1.
```python
def paginate(request, post_list):
    paginator = Paginator(post_list, settings.COUNT_POSTS)
    page_number = request.GET.get('page')
    page_obj = paginator.get_page(page_number)
    return page_obj

// вынес кол-во выводимых постов на страницу в константу COUNT_POSTS
```

2.
```python
# id настроек отчета
FILE_SETTINGS_ID = '0xC87484262E951AD0757A18766A3EF08D'

// вынес id настроек отчета в константу, так как встречался более 1 раза в даге airflow
```

3.
```python
# файл с новыми точками доставки для загрузки
DP_FILE_NAME = 'new_dp_client.csv'

// вынес наименование файла с новыми точками доставки в константу, так как в коде встречался более 1 раза в даге airflow
```

4.
```python
# значение погрешности при учете округления
ERROR_VALUE = 0.5

// вынес значение в константу, так как может измениться
```

5.
```python
# максимальное кол-во месяцев для загрузки исторических данных
MAX_MONTHS_HISTORY = 5

// дописал префикс MAX
```

6.
```python
# спец-код точки доставки для бонусных документов
BONUS_DP_CODE = 'free_product'
```

7.
```python
# Координаты точки для проверки времени суток
CHECK_LAT = 14.641980
CHECK_LONG = -90.513240

// вынес координаты в константы для удобства будущей корректировки и читаемости
```

8.
```python
# Цвет фона
BACKGROUND_COLOR = "#B1DDC6"

// вынес цвет фона окна в константу
```

9.
```python
# число Пи
PI = 3,14
```

10.
```python
# период действия токена
ACCESS_TOKEN_LIFETIME = timedelta(days=1)
```

11.
```python
# директория входящих файлов с отчётами
INBOX_PATH = os.path.join(BASE_PATH, 'data', 'inbox')

// добавил префикс INBOX_
```

12.
```python
# наименования столбцов с датами в промо-отчёте
PROMO_REPORT_DATE_COLUMNS = ['doc_date', 'date_shipment', 'date_price']

// вынес для "параметризации" программы
```