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
