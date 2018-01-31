# Getting Started with Django 2

## Estudo Django 2


* Instalar o virtualenvwrapper
$ sudo apt-get install virtualenvwrapper
--------------------------------------------------------------------------------

* Configurar o virtualenvwrapper

** procurar os arquivo virtualenvwrapper.sh
```
$ locate virtualenvwrapper
/usr/share/virtualenvwrapper/virtualenvwrapper.sh
```

** tornar executável
$ sudo chmod +x virtualenvwrapper.sh

** executar
./virtualenvwrapper.sh

** reiniciar o bash
$ exit
e abra outro
--------------------------------------------------------------------------------

* Criar ambiente virtual com o python3 como padrão
$ which python3
/usr/bin/python3

$ mkvirtualenv -p /usr/bin/python3 django2
(django2)$ 
--------------------------------------------------------------------------------

* Instalar o Django:
$ pip install django==2.0.1
Downloading Django-2.0.1-py3-none-any.whl (7.1MB)
--------------------------------------------------------------------------------

* Criar um repositório no github:
No site do github: ‘New repository’
siga 
--------------------------------------------------------------------------------
