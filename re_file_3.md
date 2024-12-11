# Django админка. Приложения, модели, миграции, superuser. Кастомизация. Видео №2
Django в комплекте имеет кучу всего встроенного для отображения информации.
И многое (базовое) нам даже не придется описывать. Этим Django и прекрасен.

Запустим наше приложение командой `python manage.py runserver`
После запуска сервера увидим в терминале красными буквами нам пишут (мы и раньше это видели)

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

Это значит у нас имеются 18 не примененных миграций, и нам предлагают выполнить эти миграции.
Даже предлагают нам команду для применения миграций `python manage.py migrate`
Нам даже говорят что не применены миграции для приложений admin, auth, contenttypes, sessions.
Все эти приложения идут в комплекте с django, они установленны по умолчанию,
их можно найти перейдя в файл по пути (django-todo-suren/todo_manager/todo_manager/settings.py)
и тут найти переменную (список) INSTALLED_APPS
Там немного больше приложений указано, но для некоторых нет никаких миграций 

Пока мы их не применим миграции, наш проект может работать неполноценно.
До того как применять миграции мы можем посмотреть, что вообще в них (миграциях)
собираются делать. Для начала посмотрим на список миграций, для этого обратимся к
документации [link](https://docs.djangoproject.com/en/5.1/topics/migrations/)
Одна из команд это `migrate` именно ее нам предлагают выполнить
есть еще `showmigrations` то есть показать какие в этом проекте есть миграции и их статус (выполнена или нет)

Выполним 

`python manage.py showmigrations`

В терминале нам выдадут такой ответ.
```terminal
admin
 [ ] 0001_initial
 [ ] 0002_logentry_remove_auto_add
 [ ] 0003_logentry_add_action_flag_choices
auth
 [ ] 0001_initial
 [ ] 0002_alter_permission_name_max_length
 [ ] 0003_alter_user_email_max_length
 [ ] 0004_alter_user_username_opts
 [ ] 0005_alter_user_last_login_null
 [ ] 0006_require_contenttypes_0002
 [ ] 0007_alter_validators_add_error_messages
 [ ] 0008_alter_user_username_max_length
 [ ] 0009_alter_user_last_name_max_length
 [ ] 0010_alter_group_name_max_length
 [ ] 0011_update_proxy_permissions
 [ ] 0012_alter_user_first_name_max_length
contenttypes
 [ ] 0001_initial
 [ ] 0002_remove_content_type_name
sessions
 [ ] 0001_initial
```
Это все миграции, которые есть у нас в проекте. У нас не применена ни одна
миграция, там нет галочек, напротив тех которые выполнены.
У каждой миграции есть ее номер и короткое название, также подписано
приложение к которому относятся эти миграции.

Django приложения идущие в комплекте, написаны на самом django, и работают
по правилам этого самом django. Если уже созданы некоторые модели, а потом
внесли некоторые изменения, то делаются отдельные миграции, что бы была 
возможность, при обновлении, версии, другим тоже получить те же самые 
изменения. В большом проекте может быть большое количество этих миграций, 
и это нормально.

Перед применением миграций, разберемся куда мы их применяем, а можно 
обратить внимание что, при создании проекта, автоматически создается 
файл, с базой данных SQLite (db.sqlite3). В django без установки дополнительных
пакетов есть поддержка этой базы данных, она очень проста в использовании.
В документации можно найти информации и по подключению PostgreSQL(самая полноценная поддержка в django), MySQL,
Можно найти, где устанавливаются правила подключения к базе данных, это
делается в файле (django-todo-suren/todo_manager/todo_manager/settings.py).
В этом файле есть переменная (словарь) DATABASE в которой указано.
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
Ссылка на документацию о базах данных [link](https://docs.djangoproject.com/en/5.0/ref/settings/#databases)
Соответственно django при старте, автоматически, создает этот файл, и
подключаясь к указанной базе, проверяет применены ли существующие миграции.

## Применение миграций
Применим наконец наши миграции командой
`python manage.py migrate`

Увидим такой вывод в консоль
```terminal
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```
Видим что все OK значит миграции применены.

Если теперь выполнить команду `python manage.py showmigrations`

В терминале нам выдадут такой ответ.
```terminal
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
 [X] 0008_alter_user_username_max_length
 [X] 0009_alter_user_last_name_max_length
 [X] 0010_alter_group_name_max_length
 [X] 0011_update_proxy_permissions
 [X] 0012_alter_user_first_name_max_length
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
sessions
 [X] 0001_initial
```
То есть мы видим что все миграции выполнены. И теперь можно проверить 
содержимое базы данных db.sqlite3

Если открыть базу и найти там таблицу django_migrations то, в ней можно 
увидеть все примененные сейчас миграции и когда они применены.

Однако особой надобности нет, в просмотре другими программами содержимого
базы данных, так как у Django есть своя встроенная админка

## Использование админки в django.
Админка позволяет, без проблем, смотреть что у нас есть в базе данных, а также, 
редактировать все что может нам понадобится в ходе разработки.

Что бы попасть в "админку" нужно к основному адресу сайта добавить /admin
В нашем случае http://127.0.0.1:8000/admin

Перейдя на эту страницу (Админку) мы увидим вход в django админку (Django administration), 
нас попросят ввести username и password. 
Так как мы только что создали проект, и еще не делали никаких пользователей,
очевидно что и пользователя никакого в базе еще не существует
Мы можем создать пользователя через терминал, для этого есть команда.

`python manage.py createsuperuser` или `django-admin createsuperuser`
это одна и та же команда как мы раньше видели.
Нас спросят, что мы хотим сделать, какого пользователя (egor)
адрес почти, и пароль (1)
И соответственно пользователь создастся.

```terminal
Username (leave blank to use 'egor'): 
Email address: afa@mail.ru
Password: 
Password (again): 
This password is too short. It must contain at least 8 characters.
This password is too common.
This password is entirely numeric.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
```

Теперь мы сможем с как супер юзер зайти в Админку. superuser это пользователь 
у которого есть все права на использование всего без ограничений.
Там мы можем сменить пароль, там уже готовый интерфейс, что бы это сделать.
Можем видеть там Authentication and Authorization можно создавать, группы (groups),
и пользователей (users). Так же есть фильтрации для удобного поиска и сортировки.

Если мы вернемся на главную страницу HOME сможем увидеть недавние действия (resent actions)
Можно перейти на каждое из действий и посмотреть что там у нас произошло, то есть
Django все действия записывает, и вот так удобно может нам предоставлять.

Эта Админка нужна не только для встроенных в django инструментов, но и для наших 
самостоятельных моделей, а очень удобно через Админку управлять моделями, можно
в некоторых случаях даже не писать свой сайт (свой бекенд), а через эту Админку сделать 
управление содержимым.

## Создание нового приложения в нашем проекте
Мы же будем делать, как рекомендует документация, а именно, будем писать наше
django приложение с самого начала. И так создадим наше новое приложение, сейчас
у нас есть проект todo_manager, и в этом проекте мы создадим новое приложение.






