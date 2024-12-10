Um projeto Flask para cadastro de eventos, com autenticação de usuários e integração com banco de dados SQLite.

## Requisitos

Certifique-se de ter instalado:

- Python 3.7 ou superior
- pip (gerenciador de pacotes do Python)
- Virtualenv

---

## Configurando o Ambiente

### 1. Clonando o Repositório
```bash
git clone <URL_DO_REPOSITÓRIO>
cd <PASTA_DO_PROJETO>
```

### 2. Criando o Ambiente Virtual
Dentro do diretório do projeto, execute:

python3 -m venv venv


### 3. Ativando o Ambiente Virtual
#### macOS/Linux:
source venv/bin/activate

#### Windows (PowerShell):
.\venv\Scripts\activate

##### Quando ativado, o prompt do terminal exibirá (venv) antes do nome do diretório atual.

### 4. Instalando Dependências
Com o ambiente virtual ativado, instale as dependências necessárias:

pip install flask flask_sqlalchemy flask_login flask_bcrypt

Para salvar as dependências no arquivo requirements.txt (caso não esteja criado):

pip freeze > requirements.txt


### 5. Banco de Dados
Certifique-se de que a pasta instance existe no diretório do projeto. Caso contrário, crie-a manualmente:

mkdir instance

O banco de dados será criado automaticamente ao iniciar o projeto.

### 6. Executando o Projeto

Com o ambiente virtual ativado, inicie o servidor Flask:
python app.py

Acesse o projeto no navegador em: http://127.0.0.1:5000

******************************************************************************

## Instalando o Projeto em Outro Local
### Para facilitar a execução do projeto em outros ambientes:

Clone o repositório e entre no diretório.
Crie um ambiente virtual:
python3 -m venv venv

### Ative o ambiente virtual:
macOS/Linux: source venv/bin/activate
Windows: .venv\Scripts\activate

### Instale as dependências do requirements.txt:
pip install -r requirements.txt

### Execute o projeto:
python app.py


## Estrutura do projeto:

.
├── app.py                 # Arquivo principal com o código do Flask
├── templates/             # Arquivos HTML
│   ├── index.html
│   ├── sobre.html
│   ├── register.html
│   ├── login.html
│   ├── eventos.html
│   ├── agenda.html
├── static/                # Arquivos estáticos (CSS, JS, imagens)
├── instance/              # Banco de dados SQLite
├── requirements.txt       # Dependências do projeto
└── venv/                  # Ambiente virtual (não versionado)


