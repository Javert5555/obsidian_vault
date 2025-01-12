python -m venv venv (python3 -m venv venv - для линукс)  - создаём виртуальное окружение (изолированное от внешней среды).
Команда `deactivate` - выйти из виртуальной среды.

`setx /M PATH "%PATH%;C:\Users\user\AppData\Roaming\Python\Python311\Scripts"`

 Запустить сервер на порту 4000 с ip адресом 127.0.0.1
``` bash
python .\manage.py runserver 127.0.0.1:4000
```
