# Mão na massa
 ### Criando ambiente virtual

-   python -m venv 'nome, convensão *.venv*'
-   Ativar ambiente virtual, acesse a pasta e selecione o arquivo *Activate*
    -   Pode ser necessário desativar uma política de segurança
        - Execute: **Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass** 
    -   Ex: cd .\.venv\Scripts\activate
- Instale o django com o ambiente ativado
    -   pip install django
    -   Para listar as dependências use o seguinte comando
        -   pip freeze
        -   Cria um arquivo listando as dependências
            -   pip freeze > requirements.txt
        -   Vai listar as dependências e versions, *importante sempre que instalar uma nova dependência, executar o comando para atualizar no arquivo requiremets.txt*

-   Listar todos os comando Django
    -   django-admin help



### Iniciar o projeto

-  django-admin startproject *nome do projeto*
    -   Padrão de nome: *setup .*
        -   O ponto no final evita que seja criado outra pasta setup dentro da primeira

#### Time Zone e Língua

-   Modifique o arquivo
    -   settings.py

    ```Python

        LANGUAGE_CODE = "pt-br"
        TIME_ZONE = "America/Sao_Paulo"

    ```

### Variáveis de ambiente

-   pip install python-dotenv
- Crie um arquivo *.env* onde será guardando informações e variáveis de ambiente que não podem ir para produção
    -   *gitignore.io* possui um documento com o que deve ser ignorado.

-   Importa o dotenv
-   Chama a função load_dotenv
```python
  from dotenv import load_dotenv
  load_dotenv()  

  SECRET_KEY = str(os.getenv("SECRET_KEY"))
```
-   Desta forma a minha chave fica no arquivo *.env* separado, utilizando depois o *.gitignore* consigo deixar o arquivo *.env* separado ao subir para o github.
 **.gitignore** possui algumas vantagens para commits descuidados, existe uma padronização dentro do sistema do Git que checa todos os nomes de arquivos e pastas presentes dentro de um arquivo chamado .gitignore e os exclui de sua análise para a adição no commit.




#### venv

- É o ambiente virtual “padrão” do Python e sua grande vantagem é já vir instalado como um módulo na linguagem a partir da versão 3.3.
- Se trata de um subset (parte menor) da ferramenta virtualenv.

#### Virtualenv

- Ferramenta feita especificamente para a criação de ambientes virtuais e precede a criação da venv, sendo um superset (parte maior) dela.
- Algumas de suas principais vantagens sobre a venv são:
  - Maior velocidade, graças ao método app-data seed;
  - Pode criar ambientes virtuais para versões arbitrárias do Python instaladas na máquina;
  - Pode ser atualizado utilizando a ferramenta pip;
  - Possui uma Programmatic API, capaz de descrever um ambiente


### Versionamento com GIT

-   Criar repositório no GITHUB
-   Iniciar git no ambiente. *GIT deve estar instalado*
-   Comandos
    -   git init
    -   git add .
    -   git commit - "Nome do projeto"
    -   git remote add origin "link do github"
    -   git push origin master

[Git e Github: os primeiros passos nessas ferramentas](https://www.alura.com.br/artigos/o-que-e-git-github)

### App e Projeto

-   python manage.py help 
    -   Comandos do manage.py
1.  StartApp
    1.  App é uma parte da aplicação, uma funcionalidades, tela, uma pequena parte, etc...
    2. **Criando um App**
    ```powershell
    python manage.py startapp galeria
    ```
    3.  Quando criamos um app é necessário vincular ao projeto, isso é feito no arquivo settings.py -> INSTALLED_APPS em setup
    
    ```python
    INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "galeria",
    ]
2.  startproject
    1. Um projeto em django pode conter diversos App dentro dele.
    

### View e URLs

-   Views.py é o arquivo responsável por renderizar na tela e o conteúdo delas, isso quando é feito uma requisição

1.  Trabalhando com *HttpResponse* e *funções*

    1.  ```python
        Arquivo views.py
        from django.http import HttpResponse

        def index(request):
            return HttpResponse("Olá mundo!")
        
    No arquivo views.py, importei o *HttpResponse* e criei uma função que está retornando uma String, depois no arquivo *urls.py*

     2.  Importando a função no arquivo *urls.py* e criando a rota para quando for feito uma requisição
    
        ```python

            from galeria.views import index
                urlpatterns = [
                    path("admin/", admin.site.urls),
                    path("", index)]
                
        
2.  Isolando URLs

-   Crie um arquivo urls.py dentro de cada APP
    -   Crie import o que for necessário, como o *path* presente no arquivo de urls padrão.
    -   Crie uma lista que será usada para armazenar os *paths*

    ```python
    from django.urls import path
    from galeria.views import index

    urlpatterns = [
        path("", index)
    ]
    ```
-   Agora é necessário deixar o *urlpatterns* disponível no *urls* de settings na pasta setup
    -   import o *include* e crie um *path* com o nome do seu app + urls do app. Ex: meu app se chama galeria, então fica da seguinte forma.

    ```python
        from django.urls import path, include

        urlpatterns = [
            path("admin/", admin.site.urls),
            path("", include("galeria.urls")),
            ]  
    ``` 
**Desta forma todas as urls do app fica no arquivo urls.py do App** pode ser feito em todas as Apps


3.  vinculandos os **templates** no projeto
    -   Dentro de settings.py existe uma lista chamada *TEMPLATES*
    e existe outra lista dentro chamada **DIRS** que é o o local que devemos passar o diretório dos templates. 

    ```python

     "DIRS": [
            os.path.join(BASE_DIR, "templates")
        ],
       #Utilizando OS.path.join, o BASE_DIR é o diretório raiz da aplicação e o nome da pasta que criamos onde ficará os templates, no exemplo nomei como "templates"  

    ```
    -   Usando o método *render* consigo dizer para view qual o arquivo que eu quero que ele renderize.

    ```python
        def index(request):
        return render(request, "index.html")
    ```
    -  Podemos criar uma pasta para cada template que criamos, deixando mais organizado nossa estrutura.  
        EX: **templates/galeria > index.html**

    -   Importando os arquivos estáticos(css/img dentro outros)
    -   Primeiro crie uma pasta em **setup > static** com nome de *static*
    -   depois em settings.py configure crie e configure as seguinte lista e variável

    ```python

        # AQUI INDICAMOS ONDE TODOS OS ARQUIVOS ESTÃO
        STATICFILES_DIRS = [
            os.path.join(BASE_DIR, 'setup/static')
        ]

        # AQUI INDICAMOS ONDE O PYTHON VAI COLETAR OS ARQUIVOS PARA IMPLEMENTAR
        STATIC_ROOT = os.path.join(BASE_DIR, "static")
   
**Dentro da pasta static criada em setup é onde se deve manter os arquivos se estilos e arquivos estáticos**
    -   É necessário executar um comando no terminal para que o python busque, endereçe e utilize esses arquivos
    -   Execute o seguinte comando:

    ```shell
        python manage.py collectstatic

    ```
4.  URL name

Quando digitamos "localhost:8000/imagem/" no navegador, a página de imagem é renderizada, porque o Django reconhece sua existência. Porém, quando estamos na página principal e clicamos na imagem, que deveria nos redirecionar para a segunda página, nos deparamos com uma página de erro.

Isso acontece porque a imagem está nos mandando para "imagem.html". Queremos acessar a página de imagem quando clicarmos nas imagens da página principal. Na linha 55 do código "index.html", encontramos o href que nos manda para "imagem.html".

Vamos indicar as URLs que já possuímos registradas em path. Vamos inserir o caminho que queremos no arquivo *"galeria > url.py"*. No último path, passaremos name='imagem'). De volta a "index.html", podemos remover "imagem.html do href da linha 55.

No lugar dele, escreveremos a função usando código Django:

```html
    <a href="{% url 'imagem' %}">
```
Depois disso, voltamos ao navegador, recarregamos a página e clicamos na primeira imagem. Isso fará com que o caminho funcione. Na segunda imagem, também precisaremos indicar o caminho <a href="{% url 'imagem' %}">. Seguiremos esse mesmo procedimento para as todas as imagens.



Para realizar as alterações, vamos acessar "templates/galeria > index.html". Vamos adicionar o código **{% load static %}**, que solicita o carregamento dos arquivos estáticos, à primeira linha do arquivo.

Vamos carregar, inicialmente, o arquivo "style.css", o caminho para encontrá-lo é "static > styles > style.css". Na linha 13 do código, vamos adicionar **{% static após o href e %}** após o fechamento das aspas, o que identifica um arquivo estático a ser subido.

Também vamos colocar **styles/style.css** entre aspas simples e levar as aspas duplas para depois do fechamento das chaves:

```html
<link rel="stylesheet" href="{% static '/styles/style.css' %}">
```



### Carregandos as imagens          

Para todos os arquivos estático, precisaremos adicionar **{% static', antes do caminho do arquivo, e ' %}** depois.

```html
<img src="{% static '/assets/logo/Logo(2).png'  %}" alt="Logo da Alura Space />
```
### Carregando outra pagina ao seleciona uma das imagens 

O nome que aparece na URL, quando tentamos carregá-la, é "imagem.html". Vamos acessar a pasta "templates/galeria" e criar um novo arquivo, chamado "imagem.html". Vamos pegar o conteúdo de "imagem.html" do conteúdo disponibilizado pelo instrutor.


Vamos informar isso criando uma nova função em "galeria > views.py". Será a função imagem, que receberá como argumento request e retornará render(request, 'galeria/imagem.html'):

```python
    from django.shortcuts import render

    def index(request):
        return render(request, 'galeria/index.html')

    def imagem(request):
        return render(request, 'galeria/imagem.html')
```

Ainda precisamos criar a rota para que isso funcione. Para fazer isso, acessaremos "galeria > urls.py", o arquivo que cuida das rotas específicas da galeria. Nela, adicionaremos uma rota com o caminho 'imagem/', imagem. Em from galeria.views, vamos importar imagem além de index:

```python
   from django.urls import path
    from galeria.views import index, imagem

    urlpatterns = [
        path("imagem/", imagem, name="imagem"),
        # # se no href do index.html estiver "imagem" ele vai encontra ou se estiver "imagem_html, ele também vai encontrar"
        # path("imagem.html", imagem, name="imagem_html")
    ]
```

### BASE, DRY e PARTIALS

Criando um arquivo separado chamado *base.html* que terá código reutilizável
DRY, "Don't Repeat Yourself", que significa "não seja repetitivo", em português. Essa ação significa não permitir a existência de código duplicado. É importante que cada parte seja isolada.
-   Vamos até a pasta "templates/galeria". Nela criaremos, o arquivo "base.html". Ele vai receber toda a estrutura principal do nosso HTML. Em outras palavras, os trechos de código semelhantes em **"index.html"** e "imagem.html".
-   Por isso, vamos selecionar todo o código, de <body> para cima, copiá-lo com um "Ctrl + C" e colá-lo no arquivo "base.html". Depois vamos fechar a tag <body> e a tag <html>. Agora vamos voltar ao arquivo "index.html" e excluir todo o trecho de código que acabamos de copiar. Depois, faremos o mesmo com o arquivo **"imagem.html"**.
-   Agora esse conteúdo está isolada no arquivo "base.html". Precisamos, portanto, utilizar o Django para passar esse conteúdo para os arquivos **"index.html"** e "imagem.html". Para isso, vamos passar **{% extends 'galeria/base.html %}**. Na linha abaixo, vamos fazer o carregamento dos arquivos estáticos com **{% load static %}**. Isso vale para os dois arquivos:

```python

    {% extends 'galeria/base.html' %}
    {% load static %}

```

Agora vamos fazer o inverso: precisamos passar o conteúdo do <body> de "index.html" para "base.html". Faremos isso usando as propriedades **block content** e **endblock**:

```html
    <body>
        {% block content %}{% endblock %}
    </body>
```

De volta a **"index.html"**, vamos informar onde começam os blocos logo abaixo dos carregamentos que fizemos nas duas primeiras linhas. Vamos inserir o block content. Também vamos indicar om final do bloco, com endblock:

```python
    {% extends 'galeria/base.html' %}
    {% load static %}
    {% block content %}
        ##HTML
    {% endblock %}
```

-   O arquivo **"base.html"** serve para reunir as estruturas principais da aplicação. Funcionalidades, botões e menus de navegação não são enviados para esse arquivo. Dentro de **"templates/galeria"**, criaremos a pasta **"partials"**. Dentro dela, criaremos o arquivo **"_footer.html"**, que receberá as estruturas.
-   Vamos acessar "templates/galeria > index.html" e recortar, com "Ctrl + X", o código entre as tags <footer>. Vamos colá-lo no arquivo **"_footer.html"**, que tem arquivos estáticos para carregar. Por isso, vamos passar **load static** na primeira linha do código:
    -   Padrão de nomenclaturas
        -   > Pasta: **partials**
        -   > Arquivo: **_nomeDoArquivo.html**

```python
    {% load static %}
        <footer class="rodape">
            ...
        </footer>
```

-   Removemos tudo relacionado ao footer dos arquivos.
-   Agora no **base.html** faremos a inclusão do 