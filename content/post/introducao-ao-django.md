+++ 
title = "Introdução ao Django"
date = 2021-03-08T23:49:21-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Conceitos básicos para entender o Django"
categoria = [
    "Programação",
]
+++
## Sobre

O Django é um framework  python para a criação de aplicações web, ele contém um conjunto de componentes que auxiliam no desenvolvimento rápido e design limpo e pragmático.

A arquitetura do django possui como padrão de projeto o MTV (Model, Template, View), que servem para:

- **Model**: Modelo do banco de dados para o projeto.
- **Template**: Páginas para visualização de dados. Normalmente, é aqui que fica o HTML que será renderizado nos navegadores.
- **View**: Lógica de negócio. É aqui que determinamos o que irá acontecer em nosso projeto.

É semelhante a arquitetura MVC, o templeta do django é equibalente a view, já a view do django seria equivalente ao controller.

## Arquivos

### `manage.py`

É um utilitário de linha de comando que permite interagir com o projeto e app django de várias maneiras. 

### Projeto

O projeto é uma coleção de configurações e apps para um site.

Dentro da pasta do projeto do django temos os seguintes arquivos:

### `__init__.py`

Arquivo vazio que indica ao python que este diretório deve ser considerado um pacote python.

### `settings.py`

Arquivo de configuração do projeto django, aqui você adiciona os apps, banco de dados, etc.

### `urls.py`

Arquivo onde declara as urls e rotas dos apps que estão relacionados ao respectivo projeto django. Por padrão tem a rota para o `admin.py` declarada

### `asgi.py`

Ponto de integração para servidores web compatíveis com ASGI usado para servir seu projeto. Útil para quando for hospedar o projeto na nuvem.

### `wsgi.py`

Ponto de integração para servidores web compativeis com WSGI, protocolo que serve http. Útil para quando for hospedar o projeto na nuvem.

### Aplicação

O app é uma aplicação que realiza uma determinada função.

Observação: Um projeto pode conter muitos apps e um app pode estar em múltiplos projetos

Dentro da pasta de app do django temos os seguintes arquivos:

### `__init__.py`

Arquivo vazio que indica ao python que este diretório deve ser considerado um pacote python.

### `admin.py`

É o arquivo para registrar a área administrativa do site, nele tem um crud do modelo do banco de dados da aplicação que está em `model.py`. Para ser acessado tem que criar uma conta admin com o comando `python3 manage.py createsuperuser`

### `apps.py`

Arquivo responsável pela configuração do app do projeto django.

### `migrations/`

As migrations estão relacionadas aos banco de dados, é a maneira do django de propagar as alterações feitas em seus modelos no `models.py` (adicionando um campo, excluindo um modelo, etc.) em seu esquema de banco de dados.

### `models.py`

Arquivo responsável por definir os modelos da aplicação. Normalmente cada modelo representa uma tabela a ser criada no banco de dados.

- Todo modelo é uma classe Python, subclasse de `django.db.models.Model` .
- Cada atributo do modelo representa uma coluna do banco de dados.
- Django fornece um api que cria automaticamente as querys SQL.

Exemplo de como fica no django:

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

Nesse caso ficaria representado como o seguinte comando SQL:

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

### `tests.py`

Arquivo responsável por definir as regras de testes da aplicação de maneira automatizada, isto é, você cria um conjunto de testes uma vez, e então conforme faz mudanças em sua aplicação, você pode checar se o seu código continua funcionando como você originalmente pretendia, sem ter que gastar tempo executando o teste manualmente.

Confira mais sobre testes na [documentação](https://docs.djangoproject.com/pt-br/3.1/intro/tutorial05/).



### `views.py`

Arquivo responsável por renderizar a página dinâmicamente. Utiliza-se uma lógica para representar os valores em um template. que é comumente um código html para representar os dados.

É basicamente o arquivo responsável por definir as regras de negócio do app.

Exemplo de uma view que retorna a data e hora corrente em html:

```
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

## Comando essenciais

Ver todos comandos do django:

`django-admin help`

O primeiro passo é criar um projeto:

`django-admin startproject <nome_do_projeto> .`

```
django-admin startproject projetodjango .
```

Isso criará um diretório, que é apresentada desta forma: 

- init.py
- settings.py
- urls.py
- wsgi.py

Criar uma aplicação (app):

`python3 manage.py startapp <nome_app>`

```
python3 manage.py startapp appdjango
```

Isso criará um diretório, que é apresentada desta forma: 

- init.py
- admin.py
- migrations/init.py
- models.py
- tests.py
- views.py

Para criar as tabelas do banco de dados através do django devemos utilizar os 2 comandos abaixo:

Criar uma lista de coisas que queremos migrar para o banco:

O comando makemigrations cria novas migrações com base nas alterações detectadas nos modelos.

```
python3 manage.py makemigrations
```

Inserir os arquivos do migrations no banco de dados:

O comando migrate sincroniza o estado do banco de dados com o conjunto atual de modelos e migrações.

```
python3 manage.py migrate
```

Carregar arquivos estáticos:

```
python3 manage.py collectstatic
```

Criar usuário admin:

`python3 manage.py createsuperuser`

```
python3 manage.py createsuperuser
```

Iniciar o servidor de desenvolimento:

Por padrão é o endereço localhost (127.0.0.1) na porta 8000, para selecionar outra porta basta digila-la como argumento após o runserver.

```
python3 manage.py runserver
```

## Preparando o ambiente

Instalar o python3

```
sudo apt-get install python3
```

Instalar o pip, o gerenciador de pacotes do python

```
apt-get install python3-pip
```

Instalar o ambiente virtual (venv)

```
sudo apt-get install python3-venv
```

Através deste módulo de python, contendo seus próprios diretórios, arquivos binários, versões e módulos independentes do sistema operacional.

Utilizando o módulo `venv`, podemos separar as dependências de cada projeto.
Desta forma, cada projeto possui suas próprias dependências, não precisando utilizar os módulos no escopo global.

Criar o ambiente virtual venv:

`python3 -m venv <nome do ambiente virtual>`

```
python3 -m venv env
```

Ativar o ambiente virtual no linux:
`source /<nome do ambiente virtual >/bin/activate`

```
source /env/bin/activate
```

Ativar o ambiente virtual no windows:
`<nome do ambiente virtual>\Scripts\activate.bat`

```
env\Scripts\activate.bat
```

Instalar o framework Django:

```
pip3 install django
```

Verifique a versão instalada no sistema com o comando `python3 -m django --version`

o ideal é ter a versão mais atualizada, caso já tenha a versão 2 instalar digite o comando:

```
pip3 install --upgrade django==3.1.5
```

## Hello world no Django

Primeiro passo é criar o projeto com o comando:

```
django-admin startproject projeto .
```

Agora temos que criar o app com o comando:

```
python3 manage.py startapp app
```

No arquivo `settings.py` do diretório do projeto devemos adicionar o app criado em INSTALED_APPS  alterar a linguagem, o horário da aplicação e também o local dos arquivos html para a pasta templates, que está dentro da pasta do app.

```
INSTALLED_APPS = [
    'app',
]

LANGUAGE_CODE = 'pt-br' 
TIME_ZONE = 'America/Sao_Paulo'

TEMPLATES = [
    {
        'DIRS': [os.path.join(BASE_DIR, 'app/templates')],
    },
]
```

O próximo passo é criar o arquivo `urls.py` na pasta do app.

A ideia aqui é importar todas as views relacionadas a url com o comando `path()`, tendo como primeiro parâmetro o `' '`, que simboliza a rota para a  raiz do site /. já o segundo deve ser `views.index`, que é o responsavel para atender as requisições, o terceiro é `name='index'` sendo o nome do aplicativo para ser declarado no html para acessar uma url.

`views.index`: faz referência a função index do arquivo views, que renderiza a página.

```
from django.urls import path
from . import views

urlpatterns = [
	path('', views.index, name='index')
]
```

Agora temos que modificar o arquivo `views.py` da pasta do app, é ele quem faz a manipulação de qual urls queremos devolver e exibir na tela.

Começamos com `def` para aplicar `index()` que recebe uma requisição como parâmetro. 

Dentro de `index()`, inserimos `request`. Em seguida, devolvemos com `return` a renderização do html com render, que no primeiro parâmetro recebe a requisição e no segundo o arquivo html.

Essa função basicamente recebe uma requisição http e retorna a resposta http.

```
from django.shortcuts import render

def index(request):
	return render(request,'index.html')
```

Agora temos que incluir o caminho index do app no `urls.py` do projeto.

Nesse caso quando acessar a raiz do site o django vai acessar acessar o arquivo `urls.py` do app.

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('app.urls')),
    path('admin/', admin.site.urls),
]
```

Próximo passo é renderizar um arquivo html, para isso no diretório do app criaremos a pasta `templates` com o arquivo `index.html` .

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hello world</title>
</head>
<body>
    <h1>hello world</h1>
</body>
</html>
```

Devemos executar o projeto com o seguinte comando:

```
python3 manage.py runserver
```

Para visualizar o hello world é só acessar a url [http://127.0.0.1:8000/](http://127.0.0.1:8000/), que é a porta padrão que o django abre no localhost.