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


### [Ativando os modelos](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#activating-models)

Primeiro nós precisamos dizer ao nosso projeto que o app `polls` está instalado.

Para incluir o app em nosso projeto, nós precisamos adicionar uma referência à essa classe de configuração editando o `INSTALLED_APPS`.

> Não esqueça da vírgula após adicionar cada app, para não gerar erros no projeto.

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

Rodando `makemigrations`, estamos modificando o arquivo que gera o esquema do banco de dados (migrações) ou criando um novo, caso este não exista.

```bash
(django2)$ python manage.py makemigrations polls
```

Saída do comando:

```bash
Migrations for 'polls':
  polls/migrations/0001_initial.py
    - Create model Choice
    - Create model Question
    - Add field question to choice
```

O arquivo `polls/migrations/0001_initial.py` criado na migração é um arquivo python comum e pode ser alterado caso você queira implementar algum truque para quando o Django for fazer mudanças no esquema do banco.

Rodando o comando abaixo, podemos ver o código SQL gerado e usado na migração.

```bash
(django2)$ python manage.py sqlmigrate polls 0001
```

Se quiser, pode rodar o comando abaixo para verificar se existem problemas no projeto sem executar migrações ou tocar no banco de dados.

```bash
(django2)$ python manage.py check
```

Agora, execute `migrate` para criar esses modelos de tabelas no banco de dados:

```bash
(django2)$ python manage.py migrate
```

saída do comando:

```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Applying polls.0001_initial... OK
```

O comando `migrate` pega todas as migrações que ainda não foram executadas e as aplica no banco de dados - essencialmente, sincronizando as mudanças que foram feitas nos modelos (models) com o esquema no banco de dados.

Os três passos principais para fazer modificações no model são:

* Altere seus models (no models.py).
* Execute `python manage.py makemigrations` para criar migrações para essas alterações.
* Execute `python manage.py migrate` para aplicar as alterações ao banco de dados.

Os comando são usados em separados para ajudar no versionamento do código.


### [Brincando com a API](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#playing-with-the-api)

Agora, vamos embarcar no shell interativo do Python e brincar com toda a da liberdade que a API do Django dá a você. Para invocar o shell Python, use o comando:

```bash
(django2)$ python manage.py shell
```

Nós usamos este ao invés de usar simplemente, "python", porque `manage.py` define a variável de ambiente `DJANGO_SETTINGS_MODULE`, o qual da ao Django o caminho de importação de seu arquivo `mysite/settings.py`.

Uma vez no shell, explore a API de banco de dados:

```python
>>> from polls.models import Choice, Question  # Import the model classes we just wrote.

# No questions are in the system yet.
>>> Question.objects.all()
<QuerySet []>

# Create a new Question.
# Support for time zones is enabled in the default settings file, so
# Django expects a datetime with tzinfo for pub_date. Use timezone.now()
# instead of datetime.datetime.now() and it will do the right thing.
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# Save the object into the database. You have to call save() explicitly.
>>> q.save()

# Now it has an ID.
>>> q.id
1

# Access model field values via Python attributes.
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

# Change values by changing the attributes, then calling save().
>>> q.question_text = "What's up?"
>>> q.save()

# objects.all() displays all the questions in the database.
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
```

Agora vamos adicionar um método `__str__` nas classes Question e Choice de `polls/modes.py`:

```python
from django.db import models

class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```

O gerador automático de admin também usará esse método.

Note que esses são métodos python normais. Vamos adicionar um apenas para demonstração:


```python
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

Salve essas mudanças e abra um novo shell python rodando `python manage.py shell` novamente:


```python
>>> from polls.models import Choice, Question

# Make sure our __str__() addition worked.
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# Django provides a rich database lookup API that's entirely driven by
# keyword arguments.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# Get the question that was published this year.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# Request an ID that doesn't exist, this will raise an exception.
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# Lookup by a primary key is the most common case, so Django provides a
# shortcut for primary-key exact lookups.
# The following is identical to Question.objects.get(id=1).
>>> Question.objects.get(pk=1)
<Question: What's up?>

# Make sure our custom method worked.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# Give the Question a couple of Choices. The create call constructs a new
# Choice object, does the INSERT statement, adds the choice to the set
# of available choices and returns the new Choice object. Django creates
# a set to hold the "other side" of a ForeignKey relation
# (e.g. a question's choice) which can be accessed via the API.
>>> q = Question.objects.get(pk=1)

# Display any choices from the related object set -- none so far.
>>> q.choice_set.all()
<QuerySet []>

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
>>> c.question
<Question: What's up?>

# And vice versa: Question objects get access to Choice objects.
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

# The API automatically follows relationships as far as you need.
# Use double underscores to separate relationships.
# This works as many levels deep as you want; there's no limit.
# Find all Choices for any question whose pub_date is in this year
# (reusing the 'current_year' variable we created above).
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Let's delete one of the choices. Use delete() for that.
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

Para mais informações sobre relacionamento de modelos, veja [Acessando objetos relacionados](https://docs.djangoproject.com/en/2.0/ref/models/relations/). Para mais informação em como usar underscores duplos para pesquisa usando campos da API, veja [Pesquisa com campos](https://docs.djangoproject.com/en/2.0/topics/db/queries/#field-lookups-intro). Para um detalhamento completo da API de banco de dados, veja nossa referência à [API de Banco de Dados](https://docs.djangoproject.com/en/2.0/topics/db/queries/).


### [Introdução à API do Django](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#introducing-the-django-admin)


#### [Criando um usuário administrador](https://docs.djangoproject.com/en/2.0/intro/tutorial02/#creating-an-admin-user)

Primeiro temos que criar um usuário que possa acessar o site de administração. Rode o seguinte comando:

```bash
(django2)$ python manage.py createsuperuser
```

Digite seu nome de usuário desejado e pressione enter.

```bash
Username: admin
```

Em seguida, será requisitado seu endereço de e-mail desejado:

```bash
Email address: admin@example.com
```

O último passo é digitar sua senha. Você será solicitado que digite sua senha duas vezes, a segunda vez como uma confirmação da primeira.

```bash
Password: **********
Password (again): *********
Superuser created successfully.
```


#### [Inicie o servidor de desenvolvimento](https://docs.djangoproject.com/pt-br/2.0/intro/tutorial02/#start-the-development-server)
O site de administração vem ativado por padrão. Vamos iniciar o servidor de desenvolvimento e explorá-lo.

Se o servidor não estiver sendo executado, inicie-o da seguinte forma:

```bash
(django2)$ python manage.py runserver
```

Agora, abra o navegador de internet e vá para “/admin/” no seu domínio local – ex:, `http://127.0.0.1:8000/admin/`. Você deverá ver a tela de acesso.
