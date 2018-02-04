# Getting Started with Django 2

## Estudo Django 2

1. Instalar o virtualenvwrapper

   ```bash
   $ sudo apt-get install virtualenvwrapper
   ```


2. Configurar o virtualenvwrapper

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


3. Criar ambiente virtual com o python3 como padrão

    ```bash
    $ which python3
    /usr/bin/python3

    $ mkvirtualenv -p /usr/bin/python3 django2
    (django2)$ 
    ```


4. Instalar o Django:

    ```bash
    $ pip install django==2.0.1
    ```


5. Criar um repositório no github:

    No site do github: **New repository**.

    Siga os passos indicados.
