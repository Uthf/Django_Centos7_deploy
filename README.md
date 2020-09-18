В этом файле содержится инструкция по деплою джанго проекта на сервер Centos7.


НАСТРОЙКИ

Создать файлы с локальными настройками и настройками для продакшена.
В локальные настройки добавить:
- дебаг
- хосты
- базовую директорию
- ключ
- БД
- статику
В настройки для продакшена добавить:
- изменить ключ
- DEBUG = False
- хосты (прописать локальный адрес и адрес сервера)
- добавить подключение к postgres
В основном файле настроек:
- убрать БД
- убрать хосты
- убрать ключ
- добавить разделение на локальные натройки и продкшн настройки

КОНФИГУРАЦИЯ
Создать папку config
В папкe config добавить:
- gunicorn.conf.py (веб-сервер для обработки локального ip адреса). Прописать настройки.
- supervisor.conf . Для подъема gunicorn в случае падения(перезапуск сервера, ошибки и т.д.). command содержит где располагается gunicorn, какую комманду выполнить, где конфигурационный файл gunicorn. Указать пользователя, указать директорию, которая содержит файл wsgi 
- добавить директорию для записи логов


Создать .gitignore и поместить  туда все лишнее
- настройки (локальные и продакшн)
- pycache
- БД



CentOS
Установить пакеты:
- Nginx
- supervisor
- git
- postgres
- gunicorn
- python3
- venv

Postgres:

sudo -u postgres psql #вход в БД
создать пользователя БД и дать ему права. Этот пользователь должен быть указан в Databases проекта

Виртуальное окружение:
- создать виртуальное окружение: python3 -m venv venv
- войти в виртуальное окружение: source venv/bin/activate
- клонировать проект с гита: git clone https://github.com/Uthf/Django_Centos7_deploy.git

Установить зависимости
Для работы с postgres установить библиотеку psycopg2: pip install psycopg2-binary

Передать файлы настроек из Windows в CentOS с помощью SSH (https://itsecforu.ru/2019/03/12/как-использовать-команду-pscp-в-windows/)
pscp local_settings.py minkin.german@10.20.13.223:/home/minkin.german/Django_Centos7_deploy/django_deploy_to_centos7

Сделать миграции
??? Получить модели из postgres

Обновить версию sqlite
wget https://kojipkgs.fedoraproject.org//packages/sqlite/3.8.11/1.fc21/x86_64/sqlite-3.8.11-1.fc21.x86_64.rpm
sudo yum install sqlite-3.8.11-1.fc21.x86_64.rpm

gunicorn django_deploy_to_centos7.wsgi:application --bind 10.20.13.223:8000



Полезные комманды: 
Активировать виртуальное окружение: env\scripts\activate.bat
Сохранить зависимости в файл: pip freeze > requirements.txt
Установить пакеты из файла: pip install -r requirements.txt
Для добавления моделей из БД: python manage.py inspectdb > <relative_path_to_models.py>
Проверка правильности моделей: python manage.py validate

CentOS:
ls содержимое папки
mkdir создать директорию