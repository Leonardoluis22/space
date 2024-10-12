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
Para realizar as alterações, vamos acessar "templates/galeria > index.html". Vamos adicionar o código **{% load static %}**, que solicita o carregamento dos arquivos estáticos, à primeira linha do arquivo.

Vamos carregar, inicialmente, o arquivo "style.css", o caminho para encontrá-lo é "static > styles > style.css". Na linha 13 do código, vamos adicionar **{% static após o href e %}** após o fechamento das aspas, o que identifica um arquivo estático a ser subido.

Também vamos colocar **styles/style.css** entre aspas simples e levar as aspas duplas para depois do fechamento das chaves:

```html
<link rel="stylesheet" href="{% static '/styles/style.css' %}">
```

### Carregandos as imagens          


     








