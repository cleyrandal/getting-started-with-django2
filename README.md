# Getting Started with Django 2

## Instalar o virtualenvwrapper

```bash
$ sudo apt-get install virtualenvwrapper
```


## Configurar o virtualenvwrapper

1. procurar os arquivo virtualenvwrapper.sh

   ```bash
   $ sudo updatedb

   $ locate virtualenvwrapper
   /usr/share/virtualenvwrapper/virtualenvwrapper.sh
   ```

2. tornar executável

   ```bash
   $ cd /usr/share/virtualenvwrapper

   $ sudo chmod +x virtualenvwrapper.sh
   ```

3. executar

   ```bash
   ./virtualenvwrapper.sh
   ```

4. reiniciar o bash (saia e abra outro)

   ```bash
   $ exit
   ```


## Criar ambiente virtual com o python3 como padrão

```bash
$ which python3
/usr/bin/python3

$ mkvirtualenv -p /usr/bin/python3 django2
(django2)$ 
```


## Instalar o Django:

```bash
(django2)$ pip install django==2.0.1
```


## [Writing your first Django app, part 1](https://docs.djangoproject.com/en/2.0/intro/tutorial01/#writing-your-first-django-app-part-1)


### Testar o Django

```bash
(django2)$ python -m django --version
```


### [Criando um projeto](https://docs.djangoproject.com/en/2.0/intro/tutorial01/#creating-a-project)

```bash
(django2)$ django-admin startproject mysite
```

A seguinte estrutura será criada:

```
.
├── mysite
│   ├── manage.py
│   └── mysite
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py
│       └── wsgi.py
└── README.md
```


### [O Servidor de desenvolvimento](https://docs.djangoproject.com/en/2.0/intro/tutorial01/#the-development-server)

```bash
(django2)$ python manage.py runserver
```


### [Criando o app Polls](https://docs.djangoproject.com/en/2.0/intro/tutorial01/#creating-the-polls-app)

```bash
(django2)$ python manage.py startapp polls
```

O diretório `polls` foi criado: 

```
mysite/
├── db.sqlite3
├── manage.py
├── mysite/
└── polls
    ├── admin.py
    ├── apps.py
    ├── __init__.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```


### [Escrevendo sua primeira view](https://docs.djangoproject.com/en/2.0/intro/tutorial01/#write-your-first-view)
Abra o arquivo polls/views.py e inclua o código abaixo. Se já houver algum `import`, deixe-o lá.

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Agora vamos criar o arquivo de rotas (URLconf) para o app polls:

```bash
(django2)$ vim polls/urls.py
```

E inclua o código abaixo:

```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

Estrutura do projeto mysite:

```
mysite/
├── db.sqlite3
├── manage.py
├── mysite/
└── polls
    ├── admin.py
    ├── apps.py
    ├── __init__.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    ├── urls.py
    └── views.py
```

O último passo é apontar o URLconf raiz para o URLconf do app polls. Abra o arquivo `urls.py` do projeto `mysite`.

```bash
(django2)$ vim mysite/urls.py
```

Adicione a importação `include` do pacote `django.urls`. Na lista `urlpatterns`, inclua o `path` para o app polls. Veja como deve ficar o arquivo:

```python

"""mysite URL Configuration
…
"""
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

Para ver o resultado, faça:

```bash
(django2)$ python manage.py runserver
```

E acesse o endereço abaixo pelo navegador:

```
http://localhost:8000/polls/
```


## [Writing your first Django app, part 2](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#writing-your-first-django-app-part-2)


### [Configurando o Banco de Dados](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#database-setup)

Abra o arquivo `settings.py` do projeto `mysite`:

```bash
(django2)$ vim mysite/settings.py
```

Modifique a time zone para a desejada ([List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)):

```python
TIME_ZONE = 'America/Sao_Paulo'
```

Agora rode o comando abaixo para criar as tabelas:

```bash
(django2)$ python manage.py migrate
```

O comando `migrate` olha as configurações de INSTALLED_APPS e cria qualquer tabela necessáira no banco de dados de acordo com as configurações do banco de dados em seu arquivo `mysite/settings.py` e as migrações de banco de dados fornecidas com os aplicativos (iremos cobrir isso mais tarde). Você verá uma mensagem para cada migração aplicada. Se você estiver interessado, rode o cliente de linha de comando de seu banco de dados e digite `\dt` (PostgreSQL), `SHOW TABLES;` (MySQL), `.schema` (SQLite), ou `SELECT TABLE_NAME FROM USER_TABLES;` (Oracle) para mostrar as tabelas criadas pelo Django.

Saída do comando:

```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
```


### [Criando os modelos](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#creating-models)

Agora iremos criar nossos modelos. Essencialmente, o layout do banco de dados com metadados adicionais.

Abra o arquivo `models.py`:

```bash
(django2)$ vim polls/models.py
```

E digite o código abaixo:

```python
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```


### [Ativando os modelos¶](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#activating-models)

Primeiro nós precisamos dizer ao nosso projeto que o app `polls` está instalado.

Para incluir o app em nosso projeto, nós precisamos adicionar uma referência à essa classe de configuração editando o `INSTALLED_APPS`.

```python
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

