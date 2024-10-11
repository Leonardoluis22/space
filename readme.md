# Mão na massa
 ### Criando ambiente virtual
-   python -m venv 'nome, convensão *.venv*'
-   Ativar ambiente virtual, acesse a pasta e selecione o arquivo *Activate*
    -   Pode ser necessário desativar uma política de segurança
        - Execute: Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass 
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
-   Desta forma a minha chave fica no arquivo *.env* separado, utilizando depois o *.gitignore* consigo deixar o arquivo *.env* separado ao subir para o github



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










