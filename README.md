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

E acesso o endereço abaixo pelo navegador:

```
http://localhost:8000/polls/
```
