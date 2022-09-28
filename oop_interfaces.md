3.1  
```python
class User:
    '''
    Класс для создания пользователей с ролями:
    "админ", "модератор", "пользователь".
    '''
    def __init__(self, role):
        self.role = role

    @staticmethod
    def admin(self):
        return User('admin')

    @staticmethod
    def moderator(self):
        return User('moderator')
    
    @staticmethod
    def user(self):
        return User('user')


if __name__ == '__main__':
    admin = User.admin()
    moderator = User.moderator()
    user = User.user()
```

3.2  

```python
class Report(ABC):
    '''
    Абстрактный класс/интерфейс по работе с отчётами.
    От него наследуются классы по работе с конкретными
    отчетами.
    '''
    ...
```