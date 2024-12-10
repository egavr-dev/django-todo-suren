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






