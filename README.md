Chatbot Bancário em Python com Machine Learning
Grupo
Flávio Henrique - 2595528
Laryssa Dantas Vieira - 2342968
Gabriel Diogo - 2683210
Guilherme Pedigone - 2712633
Gustavo Severiano -

Detalhes
Este projeto envolve a criação de um chat-bot de uma operadora de cartões, onde os clientes poderão interagir com o chat-bot e ver detalhes de suas contas, limites, faturas, etc.

Python
Flask
SQLite
Machine Learning
CountVectorizer
LogisticRegression
HTML, CSS e JavaScript com fetch()
O que o projeto faz
O chatbot permite:

Entrar com CPF
Criar conta
Visualizar conta
Consultar fatura
Consultar cartão
Bloquear cartão
Desbloquear cartão
Simular compra
Atualizar dados
Encerrar conta
Ver comandos
Classificar intenções com Machine Learning
Estrutura
Chat_Bot_Python/
├── README.md
├── requirements.txt
└── source/
    ├── app.py
    ├── ml_model.py
    ├── training_data.json
    ├── clientes.db
    ├── static/
    │   ├── css/
    │   │   ├── icon.png
    │   │   └── style.css
    │   └── JavaScript/
    │       └── script.js
    └── templates/
        └── index.html
Onde está o Machine Learning
A parte de Machine Learning está no arquivo:

source/ml_model.py
Ele usa:

from sklearn.feature_extraction.text import CountVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline
O modelo é treinado assim:

self.model = Pipeline(
    steps=[
        (
            "vectorizer",
            CountVectorizer(
                lowercase=True,
                strip_accents="unicode",
                ngram_range=(1, 2),
            ),
        ),
        (
            "classifier",
            LogisticRegression(
                max_iter=1000,
                class_weight="balanced",
            ),
        ),
    ]
)

self.model.fit(texts, labels)
Explicação
CountVectorizer transforma as frases em números.

Exemplo:

"quero ver minha fatura"
vira uma matriz numérica baseada nas palavras da frase.

LogisticRegression aprende a associar esses números com intenções, como:

minha_fatura
bloquear_cartao
criar_conta
visualizar_conta
Arquivo de treino
O treino fica em:

source/training_data.json
Exemplo:

{
  "tag": "minha_fatura",
  "patterns": [
    "minha fatura",
    "ver fatura",
    "consultar fatura",
    "valor da fatura",
    "quanto devo"
  ],
  "responses": [
    "Vou consultar sua fatura e os últimos lançamentos."
  ]
}
Para melhorar o chatbot, adicione mais frases em patterns e reinicie o Flask.

Fluxo do chatbot
O usuário digita uma mensagem no front-end.
O JavaScript envia a frase para /chat.
O Flask chama o modelo de Machine Learning.
O modelo retorna a intenção prevista.
O JavaScript executa a funcionalidade correspondente.
As mensagens são salvas no SQLite.
Exemplo:

Usuário: queria ver o valor que eu devo no cartão
Modelo: minha_fatura
Sistema: consulta /minha-fatura e exibe a fatura real do SQLite
Rotas principais
Página inicial
GET /
Classificação de intenção com ML
POST /chat
Exemplo de entrada:

{
  "message": "quero bloquear meu cartão",
  "cpf": "111.222.333-44"
}
Exemplo de saída:

{
  "intent": "bloquear_cartao",
  "confidence": 0.9123,
  "response": "Vou bloquear o cartão vinculado à sua conta."
}
Informações do modelo
GET /ml-info
Mostra as intenções conhecidas pelo modelo.

Login
POST /login
Criar conta
POST /cadastrar
Minha conta
POST /minha-conta
Meu cartão
POST /meu-cartao
Minha fatura
POST /minha-fatura
Bloquear cartão
POST /bloquear-meu-cartao
Desbloquear cartão
POST /desbloquear-meu-cartao
Simular compra
POST /compra
Atualizar dados
PUT /atualizar-meus-dados
Encerrar conta
DELETE /encerrar-conta
Banco de dados
O banco fica em:

source/clientes.db
Tabelas principais:

clientes
transacoes
messages
CREATE TABLE messages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    cpf_cliente TEXT,
    user_message TEXT NOT NULL,
    bot_response TEXT NOT NULL,
    intent TEXT,
    confidence REAL,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
);
Como executar no VSCode
Abra a pasta do projeto no VSCode.

No terminal, crie o ambiente virtual:

python -m venv venv
Ative no PowerShell:

.\venv\Scripts\Activate.ps1
Ou no CMD:

venv\Scripts\activate
Instale as dependências:

pip install -r requirements.txt
Entre na pasta source:

cd source
Inicie o Flask:

python app.py
Acesse:

http://127.0.0.1:5000
CPFs de teste
O banco já vem com alguns clientes:

111.222.333-44 - Ana Souza
222.333.444-55 - Carlos Lima
333.444.555-66 - Beatriz Oliveira
444.555.666-77 - Diego Mendes
Como testar o ML
Com o Flask rodando, teste mensagens diferentes:

quero ver o valor que eu devo
quanto está minha fatura?
perdi meu cartão
preciso liberar meu cartão
quero abrir uma conta
me mostra meu limite
Abra o console do navegador. O arquivo script.js mostra:

[ML] intenção: minha_fatura | confiança: 0.87
