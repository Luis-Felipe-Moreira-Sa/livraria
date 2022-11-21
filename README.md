# livraria
Projeto Django Livraria
Para criação da app django_livraria iniciamos criando uma pasta com o nome da que referência a aplicação. Lembrando que utilizaremos o ambiente virtual para criação do projeto.

Criando o ambiente virtual
mkdir django_livraria
cd django_livraria

Atualizar o sistema

sudo apt update 
sudo apt -y upgrade
Instalar o pip3

sudo apt install python3-pip
Instalar ferramentas adicionais

sudo apt install build-essential libssl-dev libffi-dev python3-dev
Instalando o env e virtualenv

sudo apt install -y python3-venv
sudo apt install python3-virtualenv
Instalando no MacOS

sudo pip uninstall virtualenv

sudo -H pip install virtualenv
Criando seu ambiente virtual. Vamos chamá-lo de generic env

virtualenv env
Criando o seu ambiente virtual no Windows

python -m virtualenv env
Criando seu ambiente virtual no MacOS

virtualenv -p python3 <desired-path>


virtualenv -p python3 env
Ative o ambiente virtual

. env/bin/activate
Instalar o framework Django:

pip install django
Caso queira desativar o ambiente virtual, na pasta

deactivate 
quit()
Criando o projeto Django Livraria
Depois de criaremos a pasta seguiremos alguns passos classícos do Django. Dentro da pasta executaremos o seguinte comando:

django-admin startproject core .
django-admin é um script que criará os diretórios e arquivos para você.

No diretório core (núcleo) do projeto será criada uma estrutura de diretórios, você também verá o o diretório do virtualenv, venv, que criamos antes.

manage.py é um script que ajuda com a gestão do site. Com ele, podemos iniciar um servidor de web no nosso computador sem instalar nada, entre outras coisas.

O arquivo settings.py contém a configurações do seu site, como a de conexão com o banco de dados.

O arquivo urls.py contém uma lista dos padrões usados por urlresolver.

Mudando as configurações
Vamos fazer algumas alterações no core/settings.py. Abra o arquivo usando o editor de código que você instalou anteriormente.

Vá até o final do arquivo e, logo abaixo da linha com STATIC_URL, adicione uma nova variável chamada STATIC_ROOT:

import os

 ...
 
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
Configurações para o Banco de Dados MySQL
sudo apt-get update
sudo apt-get install mysql-server
sudo apt-get install mysql-client

#entrar no banco de dados mysql
mysql -u root -p

#mostrar todos os bancos criados
SHOW DATABASES;

#criar um banco de dados
CREATE DATABASE livraria;

Dado que você tenha instalando o bando de dados, você deve realizar a seguinte configuração.

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'livraria',
        'USER': 'root',
        'PASSWORD': '12345',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
Erro

django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?
Solução

sudo apt-get install libpq-dev
sudo apt-get install libpq-dev python-dev

pip install auto-py-to-exe

sudo apt-get install libmysqlclient-dev
#https://stackoverflow.com/questions/5178292/pip-install-mysql-python-fails-with-environmenterror-mysql-config-not-found

pip install -U setuptools

pip install mysqclient
Referencia. https://docs.djangoproject.com/en/4.1/ref/databases/

Criando um banco de dados para a livraria
Para criar um banco de dados para o nossa livraria, vamos executar o seguinte comando no console (precisamos estar no diretório que contém o arquivo manage.py).

python manage.py migrate 
Vamos startar o servidor web com o segunte comando:

python manage.py runserver #startando o servidor
Modelos do Django
Sabemos os conceitos de classe e objeto nesse momento iremos criar um modelo para algumas classes do nosso projeto livraria

Um modelo no Django é um tipo especial de objeto -- ele é salvo em um banco de dados. Um banco de dados é uma coleção de dados. Um modelo no Django é um tipo especial de objeto -- ele é salvo em um banco de dados. Um banco de dados é uma coleção de dados. Ele é um local em que você vai salvar dados sobre usuários, suas postagens, etc. Usaremos um banco de dados chamado SQLite para armazenar as nossas informações. Este é o banco de dados padrão do Django -- e ele será suficiente neste primeiro momento.os. Ele é um local em que você vai salvar dados sobre usuários, suas postagens, etc. Usaremos um banco de dados chamado SQLite para armazenar as nossas informações. Este é o banco de dados padrão do Django -- e ele será suficiente neste primeiro momento. Ref. https://tutorial.djangogirls.org/pt/django_models/

Criando uma aplicação
Para manter tudo arrumado, vamos criar uma aplicação separada dentro do nosso projeto. É muito bom ter tudo organizado desde o início.

python manage.py startapp livraria
Depois de criar uma aplicação, também precisamos dizer ao Django que ele deve usá-la. Fazemos isso no arquivo core/settings.py -- abra-o no seu editor de código. Precisamos encontrar o INSTALLED_APPS e adicionar uma linha com 'livraria', logo acima do ].

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'livraria',
]
Criando os modelos para nossa livraria
No arquivo livraria/models.py definimos todos os objetos chamados Modelos -- este é um lugar em que vamos definir os relacionamentos entre as classes que estaram presentes na nossa livraria defindos no nosso diagrama e classes.

Vamos abrir livraria/models.py no editor de código, apagar tudo dele e escrever o seguinte código:

from django.db import models
from django.conf import settings

# Create your models here.

class Autor(models.Model):
    nome = models.CharField(max_length=200)

    def __str__(self):
        return self.nome 

class Categoria(models.Model):
    nome = models.CharField(max_length=200)
    descricao = models.TextField()

    def __str__(self):
        return self.nome 

class Livro(models.Model):
    nome = models.CharField(max_length=200)
    autor = models.ManyToManyField(Autor, blank = True) #relacionamento m-n muito-para-muitos
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, verbose_name="Categoria") #relacionamento 1-n
    codigo = models.CharField(max_length=20)
    quantidade = models.IntegerField()
    valor = models.FloatField()
    #python -m pip install Pillow
    imagem = models.ImageField(upload_to='livraria/media', blank=True)
    ano = models.IntegerField()
    descricao = models.TextField()

    def __str__(self):
        return self.nome 
ManyToManyField é a criação de um relacionamento entre tabelas muito-para-muitos. ForeignKey é a criação de um relacionamento entre tabelas 1 para muitos.

Para que possamos enviar imagens precisamos instalar a biblioteca Pillow com o comando:

python -m pip install Pillow
imagem = models.ImageField(upload_to='livraria/media', blank=True) criará um campo para anexarmos a imagem que desejamos vincular ao livro que está sendo cadastrado, para isso é necessário fazermos alumas configunrações, veremos com mais detalhes em sessões posteriores.

Criando tabelas para nossos modelos no banco de dados
O último passo é adicionar nosso novo modelo ao banco de dados. Primeiramente, precisamos fazer com que o Django entenda que fizemos algumas alterações nos nossos modelos.

python manage.py makemigrations livraria
O Django preparou um arquivo de migração que precisamos aplicar ao nosso banco de dados.

python manage.py migrate livraria
Django Admin
Para fazermos as operações de

Adicionar
Editar
Deletar
Nas tabelas Autor, Categoria e Livro que modelamos iremos inicialmente usar o admin do Django. Vamos abrir o arquivo livraria/admin.pyno editor de código e acrescentamos os códigos seguintes.

from django.contrib import admin
from .models import Categoria, Autor, Livro

admin.site.register(Categoria)
admin.site.register(Autor)
admin.site.register(Livro)
Vamos startar o servidor web

python manage.py runserver #startando o servidor
Vamos acessar a área do administrador do sistema que já vem prontinho para gente graças ao framework Django, para isso iremos usamos o segunte endereço no navegador de sua preferência:

http://127.0.0.1:8000/admin/
Para fazer login, você precisa criar um superusuário (superuser) - uma conta de usuário que pode controlar tudo no site.

python manage.py createsuperuser
Personalizando o Django Admin
A página de administração padrão do Django é ótima e é tudo o que precisamos para nossos projetos, mas às vezes podemos querer expandir o que a interface de administração padrão pode fazer. Temos muitas ferramentas no Django para personalizá-lo de acordo com nossas necessidades.

Vamos abrir novamente o arquivo livraria/admin.py no editor de código e acrescentamos os códigos seguintes.

Por padrão, o administrador exibirá os campos na visualização de detalhes na mesma ordem definida no modelo. Mas podemos mudar isso fazendo algumas edições no arquivo admin.py sem ir para models.py e mudar a ordem dos diferentes campos.

from django.contrib import admin
from .models import Categoria, Autor, Livro


class LivroAdmin(admin.ModelAdmin):

    list_display = ['nome', 'valor', 'view_name_categoria', 'get_autores']
    search_fields = ['nome'] #pesquisa por nome
    list_filter = ['ano'] #filtrar por ano

    @admin.display(ordering='view_name_categoria')
    def view_name_categoria(self, obj):
        return obj.categoria.nome

    
    def get_autores(self, obj):
        for autor in obj.autor.all():
            return autor.nome
        #return [autor.nome for autor in obj.autor.all()]



admin.site.register(Categoria)
admin.site.register(Autor)
admin.site.register(Livro, LivroAdmin)

Ref. https://docs.djangoproject.com/pt-br/4.1/ref/contrib/admin/

Quando for solicitado, insira seu nome de usuário (letras minúsculas, sem espaços), e-mail e senha. Não se preocupe por não conseguir ver a senha que está digitando - é assim mesmo. Digite a senha e aperte a tecla enter para continuar.

Depois disso, volte ao seu navegador. Faça login com as credenciais de superusuário que você escolheu; você deverá ver o painel de controle de administração do Django.

URLs
Uma URL é um endereço da web. Você pode ver uma URL toda vez que você visita um website - ela aparece na barra de endereços do seu navegador. (Sim! 127.0.0.1:8000 é uma URL! E https://djangogirls.org também é uma URL.)

Cada página na Internet precisa de sua própria URL. Desta forma, sua aplicação sabe o que deve mostrar a um usuário que abre uma URL. Em Django, usamos algo chamado URLconf (configuração de URLs). URLconf é um conjunto de padrões que o Django vai usar para comparar com a URL recebida para encontrar a resposta correta. Ref. https://tutorial.djangogirls.org/pt/django_urls/

Como funcionam as URLs em Django?
Abra o arquivo core/urls.py no seu editor de código preferido e veja o que aparece:

"""core URL Configuration

[...]
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
A URL do admin, que você visitou no capítulo anterior, já está aqui:

path('admin/', admin.site.urls),
Isso significa que para cada URL que começa com admin/, o Django irá encontrar uma view correspondente. Neste caso nós estamos incluindo várias URLs de admin de uma vez a partir de uma lista criada pelo próprio Django em admin.site.urls. Desta forma, não temos que repetir todas URLs no nosso modesto arquivo -- é mais legível e mais limpo.

Sua primeira URL no Django!
É hora de criar nossa primeira URL! Queremos que http://127.0.0.1:8000/ seja a página inicial da nossa livraria e exiba uma lista de livros.

Também queremos manter o arquivo de core/urls.py limpo, e portanto importaremos as URLS da nossa aplicação livraria no arquivo principal core/urls.py.

Vamos adicionar uma linha que importará livraria.urls Você também irá precisar alterar a linha from django.urls... porque nós estamos utilizando a função include aqui, então você precisará adicionar aquele import para a linha.

O seu arquivo core/urls.py deve agora se parecer com isto:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('livraria.urls')),
]
O Django agora irá redirecionar tudo o que entra em 'http://127.0.0.1:8000/' para livraria.urls e procurar por novas instruções lá.

Criando livraria.urls
Crie um arquivo chamado urls.py na pasta livraria/urls.py do seu projeto.

from django.urls import path
from . import views #arquivo views que ainda não utilizamos

urlpatterns = [
    path('', views.listar_livros, name='listar_livros'),
]
Como você pode ver, estamos agora atribuindo uma view chamada listar_livros à URL raiz. Este padrão de URL corresponde a uma sequência de caracteres vazia, e o resolvedor de URLs do Django irá ignorar o nome de domínio (ou seja, http://127.0.0.1:8000 /) que antecede o caminho completo da URL. Este padrão dirá ao Django que views.listar_livros é o lugar correto aonde ir se alguém entra em seu site pelo endereço 'http://127.0.0.1:8000/'.

A última parte, name='listar_livros', é o nome da URL que será usado para identificar a view. Pode ser o mesmo nome da view, mas também pode ser algo completamente diferente. Nós vamos usar URLs nomeadas mais à frente, então é importante nomearmos agora todas as URLs de nossa aplicação.

Agora vamos testar a aplicação, caso tenha interrompido o servidor você pode usar o comando para starta-lo.

python manage.py runserver #startando o servidor
Django views
Uma view é o lugar onde nós colocamos a "lógica" da nossa aplicação. Ela vai extrair informações do model que você criou e entregá-las a um template. Views são apenas funções Python um pouco mais complicadas do que aquelas que criamos no capítulo Introdução ao Python.

As views são colocadas no arquivo views.py. Nós vamos adicionar nossas views ao arquivo livraria/views.py.

from django.shortcuts import render

def listar_livros(request):
    return render(request, 'livraria/listar_livros.html', {})
Temos que criar o arquivo listar_livros.html para isso devemos criar a pasta livraria\templates, em seguida crie um diretório chamado livraria,

livraria
templates
livraria
listar_livros.html
No arquivo listar_livros.html, vamos escrever o seguinte código html.

<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <p>Seja bem vindo!</p>
        <p>Boas compras</p>
    </body>
</html>
Vamos startar o servidor web

python manage.py runserver #startando o servidor
QuerySets e ORM do Django
O que é um QuerySet?
Uma QuerySet (conjunto de busca) é, em essência, uma lista de objetos de um dado modelo. QuerySet permite que você leia os dados a partir de uma base de dados, filtre e ordene.

O que o ORM do Django faz?
O Django ORM é uma implementação do conceito de mapeamento objeto-relacional (ORM). O ORM do Django é compatível com MySQL, PostgreSQL, SQLite e Oracle.

O ORM do Django é fornecido com construções especiais de abstração que podem ser usadas para criar consultas complexas ao banco de dados. Em outras palavras, no lugar de realizar uma ação direta no banco de dados com código SQL por exemplo, utilizamos o ORM como ponte de comunicação entre o banco e a aplicação. Ref. https://www.alura.com.br/artigos/django-query-sets-e-orm

O Shell do Django
É um terminal que tem as configurações do Django já importadas, portanto, permite que você trabalhe diretamente da pasta raiz de um projeto Django.

Para entrar no sheel do Django digite:

python manage.py shell
Vamos mostrar os dados já inseridos no nosso bando de dados Sqlite.

Autor.objects.all() #mostrando todos os dados da tabela Autor. Equivalente ao SELECT * FROM Autor;
Categoria.objects.all() #mostrando todos os dados da tabela Categoria
Livro.objects.all() #mostrando todos os dados da tabela Categoria
Todas as consultas vão dar erro, pois não fizemos o import das nossas classes. Para isso digitaremos a seguinte linha:

from livraria.models import Autor, Categoria, Livro #No shell temos que fazer a referencia app livraria
Autor.objects.all() #mostrando todos os dados da tabela Autor. Equivalente ao SELECT * FROM Autor;
Pelo shell podemos inserir no banco de dados um Autor, Categoria ou Livro. Vamos inserir um Autor, para isso vamos lembrar da nossa classe Autor do nosso model.py

"""
código omito
"""
class Autor(models.Model):
    nome = models.CharField(max_length=200)

    def __str__(self):
        return self.nome 
"""
código omito
"""
Autor.objects.create(nome="Eric Matthes") #Equivalente a: INSERT INTO Autor (nome) VALUES (value1);
# INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...);
Autor.objects.create(nome="Nilo Ney Coutinho Menezes")
Autor.objects.all() #mostrando todos os dados da tabela Autor. Equivalente ao SELECT * FROM Autor;
Categoria.objects.create(nome="Matemática")
Categoria.objects.create(nome="Física")
Categoria.objects.all() #mostrando todos os dados da tabela Categoria
Para consultar os usuários existentes no sitema tempos que importar o modelo User (classe que já dada prontinha para gente pelo Django )

from django.contrib.auth.models import User
User.objects.all()
Filtrando objetos
Um recurso importante dos QuerySets é a possibilidade de filtrar.

Caso você queira fazer uma pesquisa dos Autores que tem a letra A no nome por exemplo.

Autor.objects.filter(nome__contains='A') #Equivalente a: SELECT * FROM Autor WHERE nome LIKE '%A%';
Autor.objects.filter(nome__contains='Andrew') #Equivalente a: SELECT * FROM Autor WHERE nome LIKE '%Andrew%';
Digamos que queremos encontrar um livro de determinado Autor. Para isto, usamos filter ao invés de all em Livro.objects.all(), para isso vamos lembrar da nossa classe Livro do nosso model.py

'''Código omitido'''

class Livro(models.Model):
    nome = models.CharField(max_length=200)
    autor = models.ManyToManyField(Autor, blank = True) #relacionamento m-n muito-para-muitos
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, verbose_name="Categoria") #relacionamento 1-n
    codigo = models.CharField(max_length=20)
    quantidade = models.IntegerField()
    valor = models.FloatField()
    imagem = models.ImageField(upload_to='livraria/media', blank=True)
    ano = models.IntegerField()
    descricao = models.TextField()

    def __str__(self):
        return self.nome 
Primeiramente devemos saber o id do Autor para assim fazermos uma busca na tabela livraria, pois o autor está na tabela livro, pórem como um relacionamento (ManyToManyField), então o que existe dentro da tabela livro do autor é apenas um id (número) o qual podemos identificar na tabela autor.

autor = Autor.objects.get(id=1) #vamos conseguir o autor cujo o id = 1
autor.id #será mostrado o id
autor.nome #será mostrado o nome do autor sujo o id é 1
Livro.objects.filter(autor__id=autor.id) #Selecione todos os livros do autor com o id indicado
Ordenando os objetos
Podemos ordenar uma determinada consulta de uma tabela, como a consulta em SQL: SELECT * FROM Customers ORDER BY Country; Ref. https://docs.djangoproject.com/en/3.1/topics/db/queries/

Autor.objects.all()
Autor.objects.order_by('nome') #SELECT * FROM Autor ORDER BY nome; 
exit() #para sair do shell
Dados dinâmicos em templates
Até o momento, temos diferentes peças: o modelo Categoria, Autor e Livro estão definidos em models.py, e temos listar_livros em views.py e o template adicionado. Mas como faremos de fato para que os dados cadastrados apareçam no nosso template em HTML? Porque é isso que nós queremos: pegar algum conteúdo (modelos salvos no banco de dados) e exibi-lo de uma maneira bacana no nosso template, certo?

E isso é exatamente o que as views devem fazer: conectar modelos e templates. Vamos precisar pegar os modelos que queremos exibir e passá-los para o template utilizando a view. Em uma view, nós decidimos o que (qual modelo) será exibido em um template.

No nossa app iremos criar 3 views para mostramos as nossas categorias, autores e livros.

A primeira coisa que devemos fazer é ir ao arquivo livraria/views.py e fazer add o importe seguinte que já vimos na aula sobre QuereSets.

from livraria.models import Autor, Categoria, Livro
'''Como ele irá ficar'''
from django.shortcuts import render
from livraria.models import Autor, Categoria, Livro

def listar_livros(request):
    return render(request, 'livraria/listar_livros.html', {})
Iremos criar mais duas funções:

'''Como ele irá ficar'''
from django.shortcuts import render
from livraria.models import Autor, Categoria, Livro

def listar_categorias(request):
    return render(request, 'livraria/listar_categorias.html', {})

def listar_autores(request):
    return render(request, 'livraria/listar_autores.html', {})

def listar_livros(request):
    return render(request, 'livraria/listar_livros.html', {})
Iremos criar dois novos arquivos HTML na pasta livraria/templates/livraria.

listar_categorias.html
listar_autores.html
Teremos que criar também três novas rotas no arquivo livraria/urls.py

from django.urls import path
from . import views #arquivo views que ainda não utilizamos

urlpatterns = [
    path('', views.listar_livros, name='listar_livros'),
    path('listar_categorias', views.listar_categorias, name='listar_categorias'), #nova url
    path('listar_autores', views.listar_autores, name='listar_autores'), #nova url
]
QuerySet
Você já deve estar familiarizada com o modo que os QuerySets funcionam. Nós conversamos sobre isso no capítulo QuerySets e ORM do Django.

from django.shortcuts import render
from livraria.models import Autor, Categoria, Livro

def listar_categorias(request):
    categorias = Categoria.objects.all()
    return render(request, 'livraria/listar_categorias.html', {'categorias':categorias})

def listar_autores(request):
    autores = Autor.objects.all()
    return render(request, 'livraria/listar_autores.html', {'autores':autores})

def listar_livros(request):
    livros = Livro.objscts.all()
    return render(request, 'livraria/listar_livros.html', {'livros':livros})
Pronto! Hora de voltar para o nosso template e exibir essa QuerySet!

Templates para lista de Categorias, Autores e Livros
A primeira template que iremos construir é a de categorias livraria/templates/livraria/listar_categorias.html.

<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <h3>Lista das nossas Categorias</h3>
        {{categorias}}
    </body>
</html>
Para que possamos percorrer a lista de categorias devemos utilizar um laço de repetição, para isso usaremos o for.

<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <h3>Lista das nossas Categorias</h3>
        
        {% for categoria in categorias %}
        
            {{categoria.nome}}
            
        {% endfor %}
    
    </body>
</html>
Agora que já entendemos o procendimento de criação de templates dinâmicas podemos criar a nossa template para listar os nossos Autores. Arquivo livraria/templates/livraria/listar_autores.html.

<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <h3>Lista dos nossos Autores</h3>    
        {% for autor in autores %}
            {{autor.nome}}   
        {% endfor %} 
    </body>
</html>
Você pode modificar a queryset do aquivo views e ordenar os autores por nome.

Agora vamos modificar a template dos livros, onde iremos listar todos os livros cadastrados na nossa livraria. Arquivo livraria/templates/livraria/listar_livros.html.

<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <p>Seja bem vindo!</p>
        <p>Boas compras</p>

        {% for livro in livros %}
            Nome: {{ livro.nome }}<br/>
            Autores: <br/>
            {% for nome in livro.autor.all %}
                {{ nome }}<br/>
            {% endfor %}
        {% endfor %}

    </body>
</html>
Veja que temos dois for's em encadeados

{% for livro in livros %} # ira listar todos os livros
    {% for nome in livro.autor.all %}  # ira listar todos os autores por livro
O primeiro for irá listar todos os livros.
O segnudo irá listar todos os autores por livro, pois podemos ter mais de um autor por livro.
<html>
    <head>
        <title>Django Livraria</title>
    </head>
    <body>
        <p>Seja bem vindo!</p>
        <p>Boas compras</p>

        {% for livro in livros %}
            Nome: {{ livro.nome }}<br/>
            Código: {{ livro.codigo }}<br/>
            Ano: {{ livro.ano }}<br/>
            Valor: {{ livro.valor }}<br/>
            <img height="300" width="200" src="{{ livro.imagem.url }}">
            Autores: <br/>
            {% for nome in livro.autor.all %}
                {{ nome }}<br/>
            {% endfor %}     
        {% endfor %}

    </body>
</html>
Para que possamos exibir a imagem de cada livro cadastrado temos que utilizar a tag do HTML <img> e em seu atributo src colocar o endereço da imagem que conseguimos acessar com livro.imagem.url, a url irá localizar o caminho da imagem na pasta livraria/media.

<img height="300" width="200" src="{{ livro.imagem.url }}">

Outra configuração que deve ser adicionada é no arquivo core/urls.py, ao fim do arquivo devemos adicionar a seguinte linha que fará referência a pasta media.

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
Como o arquivo core/urls.py irá ficar:

from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('livraria.urls'))
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
Agora é startar o servidor e vermos os resultados.

python manage.py runserver #startando o servidor
Deixando tudo mais bonito
Está na hora de deixar ele mais bonitinho! Para isso, nós usaremos o CSS.

O que é CSS? Cascading Style Sheets (CSS - Folhas de Estilo em Cascata, em português) é uma linguagem utilizada para descrever o visual e a formatação de um website escrito numa linguagem de marcação (como HTML). Considere como uma maquiagem para a nossa página web. :)

Mas não queremos começar do zero de novo, né? Mais uma vez, usaremos algo que outros programadores lançaram na Internet de graça. Você sabe, reinventar a roda não é divertido.

Quick start
Vamos começar com Bootstrap, a estrutura mais popular do mundo para a criação de sites responsivos para dispositivos móveis, com jsDelivr e uma página inicial de modelo.

Ref. https://getbootstrap.com/docs/5.0/getting-started/introduction/

Iremos iniciar a estilização das páginas de forma simples utilizando apenas um link de configuração do bootstrap. Vamos introduzir na nossa página inicial no arquivo livraria/templates/livraria/listar_livros.html.

Dentro da tag head iremos coloca o link abaixo:

<link 
href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" 
rel="stylesheet" 
integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" c
rossorigin="anonymous">
A página ficará dessa forma:

<html>
    <head>
        <title>Django Livraria</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" 
        rel="stylesheet" 
        integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" 
        crossorigin="anonymous">
    </head>
    <body>
        <p>Seja bem vindo!</p>
        <p>Boas compras</p>

        {% for livro in livros %}
            Nome: {{ livro.nome }}<br/>
            Código: {{ livro.codigo }}<br/>
            Ano: {{ livro.ano }}<br/>
            Valor: {{ livro.valor }}<br/>
            <img height="300" width="200" src="{{ livro.imagem.url }}">
            Autores: <br/>
            {% for nome in livro.autor.all %}
                {{ nome }}<br/>
            {% endfor %}     
            <br>
        {% endfor %}

    </body>
</html>
