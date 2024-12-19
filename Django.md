# [[Python]]

### Создание проекта
- `python3 -m venv venv` создание виртуального окружения
- `source venv/bin/activate` активация виртуального окружения
- `pip3 install Django` установка Django
- `django-admin startproject myproject` создание структуры проекта
- `python manage.py runserver` запуск сервера
### Работа с проектом
- `python manage.py startapp app` создание приложения

> [!note]
> После создания приложения необходимо добавить его в файл settings.py

```python
INSTALLED_APPS = [
'app',
]
```

> [!note]
> Для работы с шаблонами мы создаем папку templates и регистрируем её в settings.py

`'DIRS': ['templates']` заносим название папки с шаблонами в переменную DIRS

> [!note]
> Все функции обработчики пишем в файле views, импортируем эти функции в файл urls для отображения результата по определенному адресу

### Работа с моделями
- `python manage.py makemigrations` создать миграции
- `python manage.py migrate` провести миграции в базу данных

Пример кода
```python
from django.db import models

class Post(models.Model):
	title = models.CharField(max_length=40, blank=False)
	content = models.TextField(blank=False)
	created_at = models.DateTimeField(auto_now_add=True)
```

### Админ панель
- `python manage.py createsuperuser` создать суперюзера
- `admin.site.register(Post)` регистрация модели

Отображение заголовка в админке или при выводе объекта в консоль
```python
def __str__(self): 
	return self.title
```