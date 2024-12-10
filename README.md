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


### Completando o CRUD:

foi solicitado que fizessem o complemento do projeto para que pudesse editar e deletar um evento. Segue abaixo uma explicacao do que deveria ser feito para complementar o codigo.

### 1. A dicionar a Função de Editar Evento
Backend: Rota para Editar Evento
Adicione esta rota ao seu código:

````
@app.route('/eventos/<int:evento_id>/editar', methods=['GET', 'POST'])
@login_required
def editar_evento(evento_id):
    evento = Evento.query.get_or_404(evento_id)
    
    # Verifica se o usuário atual é o criador do evento
    if evento.usuario_id != current_user.id:
        flash('Você não tem permissão para editar este evento.', 'danger')
        return redirect(url_for('eventos'))
    
    if request.method == 'POST':
        evento.titulo = request.form.get('titulo')
        evento.descricao = request.form.get('descricao')
        data_evento = request.form.get('data_evento')
        
        # Converte a data de string para datetime
        evento.data_evento = datetime.strptime(data_evento, '%Y-%m-%d').date()
        
        db.session.commit()
        flash('Evento atualizado com sucesso!', 'success')
        return redirect(url_for('eventos'))
    
    return render_template('editar_evento.html', evento=evento)
````

#### Frontend: Formulário para Editar Evento
Crie o arquivo editar_evento.html no diretório templates:

````

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Editar Evento</title>
</head>
<body>
    <h1>Editar Evento</h1>
    <form method="POST">
        <label for="titulo">Título:</label>
        <input type="text" name="titulo" id="titulo" value="{{ evento.titulo }}" required>
        
        <label for="descricao">Descrição:</label>
        <textarea name="descricao" id="descricao" required>{{ evento.descricao }}</textarea>
        
        <label for="data_evento">Data do Evento:</label>
        <input type="date" name="data_evento" id="data_evento" value="{{ evento.data_evento }}" required>
        
        <button type="submit">Salvar Alterações</button>
    </form>
</body>
</html>
````

### Adicionar a Função de Deletar Evento
Backend: Rota para Deletar Evento
Adicione esta rota ao seu código:

````
@app.route('/eventos/<int:evento_id>/deletar', methods=['POST'])
@login_required
def deletar_evento(evento_id):
    evento = Evento.query.get_or_404(evento_id)
    
    # Verifica se o usuário atual é o criador do evento
    if evento.usuario_id != current_user.id:
        flash('Você não tem permissão para deletar este evento.', 'danger')
        return redirect(url_for('eventos'))
    
    db.session.delete(evento)
    db.session.commit()
    flash('Evento deletado com sucesso!', 'success')
    return redirect(url_for('eventos'))
````

#### Frontend: Botão de Deletar
Adicione um botão de deletar na página onde lista os eventos (eventos.html ou similar):

````
{% for evento in eventos %}
    <div>
        <h2>{{ evento.titulo }}</h2>
        <p>{{ evento.descricao }}</p>
        <p>{{ evento.data_evento }}</p>
        
        <a href="{{ url_for('editar_evento', evento_id=evento.id) }}">Editar</a>
        
        <form action="{{ url_for('deletar_evento', evento_id=evento.id) }}" method="POST" style="display:inline;">
            <button type="submit" onclick="return confirm('Tem certeza que deseja deletar este evento?')">Deletar</button>
        </form>
    </div>
{% endfor %}
````
### 3. Resumo das Alterações
Editar:

Criar a rota /eventos/<int:evento_id>/editar para carregar e atualizar os dados do evento.
Criar um formulário editar_evento.html para o usuário atualizar os dados.
Deletar:

Criar a rota /eventos/<int:evento_id>/deletar para remover o evento do banco de dados.
Adicionar um botão "Deletar" no frontend, vinculado a esta rota.


### 4. Controle de Permissão
Em ambas as rotas (editar e deletar), você verificou se o usuário autenticado é o criador do evento:

````
if evento.usuario_id != current_user.id:
    flash('Você não tem permissão para editar/deletar este evento.', 'danger')
    return redirect(url_for('eventos'))
````

Isso garante que um usuário não possa modificar eventos de outro usuário.

Qualquer dúvida é só entrar em contato comigo.
