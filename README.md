<h1 align="center">🛒 API de Produtos com Node.js, Express e MongoDB</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20.19%2B-339933?logo=node.js&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/Express-5.x-000000?logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/MongoDB-7.x-47A248?logo=mongodb&logoColor=white" alt="MongoDB" />
  <img src="https://img.shields.io/badge/licen%C3%A7a-ISC-blue" alt="Licença ISC" />
</p>

<p align="center">
  API REST para gerenciar produtos de uma loja, com CRUD completo e banco de dados NoSQL.
</p>

---

## 📑 Sumário

- [Sobre o projeto](#-sobre-o-projeto)
- [Tecnologias](#-tecnologias)
- [Pré-requisitos](#-pré-requisitos)
- [Instalação](#-instalação)
- [Configuração](#%EF%B8%8F-configuração)
- [Como executar](#%EF%B8%8F-como-executar)
- [Endpoints](#-endpoints)
- [Exemplos de uso](#-exemplos-de-uso)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Como funciona por dentro](#-como-funciona-por-dentro)
- [Problemas comuns](#-problemas-comuns)
- [Limitações conhecidas e melhorias futuras](#-limitações-conhecidas-e-melhorias-futuras)
- [Prints](#-prints)
- [Autor](#-autor)

---

## 📖 Sobre o projeto

Esta é uma **API REST** feita com **Node.js** e **Express** que se conecta a um banco **MongoDB**. Ela guarda os produtos na coleção `produtos` do banco `loja` e permite fazer as quatro operações básicas de qualquer sistema, conhecidas como **CRUD**:

| Letra | Significado | Método HTTP | O que faz nesta API            |
| ----- | ----------- | ----------- | ------------------------------ |
| **C** | Create      | `POST`      | Cadastra um produto            |
| **R** | Read        | `GET`       | Lista ou busca produtos        |
| **U** | Update      | `PUT`       | Altera os dados de um produto  |
| **D** | Delete      | `DELETE`    | Remove um produto              |

> 💡 **O que é uma API REST?** É um programa que fica "escutando" pedidos (requisições) feitos por HTTP, como os que o navegador faz, e devolve respostas em formato JSON. Quem consome a API (um site, um app, o Postman) não precisa saber como o banco funciona por dentro, só precisa saber quais rotas chamar.

Projeto criado para praticar desenvolvimento de APIs e banco de dados NoSQL.

---

## 🧰 Tecnologias

| Tecnologia | Para que serve no projeto |
| ---------- | ------------------------- |
| 🟢 [Node.js](https://nodejs.org/) | Executa o JavaScript fora do navegador, no servidor |
| 🚂 [Express 5](https://expressjs.com/) | Cria o servidor web e define as rotas da API |
| 🍃 [MongoDB](https://www.mongodb.com/) | Banco de dados NoSQL que guarda os produtos em documentos JSON |
| 🔌 [Driver oficial do MongoDB](https://www.mongodb.com/docs/drivers/node/current/) | Biblioteca que conecta o Node.js ao MongoDB |

---

## ✅ Pré-requisitos

Antes de começar, você precisa ter:

- 🟢 **Node.js 20.19.0 ou superior** (exigência do driver do MongoDB 7). Para conferir a versão, rode `node -v` no terminal.
- 🍃 **Um banco MongoDB** acessível, que pode ser instalado na sua máquina ou criado de graça no [MongoDB Atlas](https://www.mongodb.com/atlas).
- 🧪 **Uma ferramenta para testar as rotas**, como Postman, Insomnia, Thunder Client (extensão do VS Code) ou o próprio `curl`.
- 🔧 **Git**, para clonar o repositório.

---

## 📦 Instalação

```bash
# 1. Clone o repositório
git clone https://github.com/caiomarcellino11/NOME-DO-REPOSITORIO.git

# 2. Entre na pasta do projeto
cd NOME-DO-REPOSITORIO

# 3. Instale as dependências (express e mongodb)
npm install
```

O comando `npm install` lê o arquivo `package.json` e baixa tudo que o projeto precisa para a pasta `node_modules`.

---

## ⚙️ Configuração

A API descobre onde está o banco de dados pela variável de ambiente **`MONGO_URI`**. Ela guarda a *string de conexão* do MongoDB. Defina essa variável **antes** de iniciar o servidor.

**🐧 Linux / 🍎 macOS:**

```bash
export MONGO_URI="mongodb://localhost:27017"
```

**🪟 Windows (PowerShell):**

```powershell
$env:MONGO_URI = "mongodb://localhost:27017"
```

**🪟 Windows (CMD):**

```cmd
set MONGO_URI=mongodb://localhost:27017
```

### Qual string de conexão usar?

| Cenário | Formato da string |
| ------- | ----------------- |
| MongoDB instalado na sua máquina | `mongodb://localhost:27017` |
| MongoDB Atlas (nuvem) | `mongodb+srv://USUARIO:SENHA@cluster.mongodb.net/` |

No Atlas, você encontra a sua string em **Database → Connect → Drivers**. Lembre de trocar `USUARIO` e `SENHA` pelos dados reais e de liberar o seu IP em **Network Access**.

> ⚠️ **Segurança:** a string de conexão do Atlas contém usuário e senha. **Nunca** coloque ela direto no código nem suba para o GitHub.

> 📝 A variável vale só para o terminal onde você a definiu. Se fechar o terminal, é preciso definir de novo.

---

## ▶️ Como executar

```bash
node index.js
```

Se tudo estiver certo, o terminal mostra:

```
conexão realizada com sucesso!
API RODANDO NA PORTA 3000
```

A API fica disponível em **`http://localhost:3000`**. Para parar o servidor, use `Ctrl + C` no terminal.

---

## 🛣️ Endpoints

Todas as rotas começam com `/produtos`.

| Método | Rota | Descrição | Precisa de corpo (JSON)? |
| ------ | ---- | --------- | ------------------------ |
| ➕ `POST` | `/produtos` | Cadastra um novo produto | Sim |
| 📋 `GET` | `/produtos` | Lista todos os produtos | Não |
| 🔍 `GET` | `/produtos/:id` | Busca um produto pelo `_id` | Não |
| ✏️ `PUT` | `/produtos/:id` | Atualiza campos de um produto | Sim |
| 🗑️ `DELETE` | `/produtos/:id` | Remove um produto | Não |

### Entendendo o `:id`

O `:id` é o **`_id`** que o próprio MongoDB cria para cada documento. Ele é um `ObjectId`, um texto de **24 caracteres hexadecimais**, por exemplo `6ae49ae84903bd8f7b574574`. Você recebe esse valor na resposta do `POST` (campo `insertedId`) e também o vê na listagem (`GET /produtos`).

### Entendendo o corpo da requisição

Nas rotas `POST` e `PUT`, os dados do produto vão no **corpo** da requisição, em JSON, com o cabeçalho `Content-Type: application/json`. Como o MongoDB não exige um esquema fixo, você escolhe os campos. Nos exemplos deste README foram usados `nome`, `preco` e `categoria`.

---

## 🚀 Exemplos de uso

### ➕ Criar produto

```bash
curl -X POST http://localhost:3000/produtos \
  -H "Content-Type: application/json" \
  -d '{"nome": "Mouse", "preco": 800, "categoria": "periferico"}'
```

**Resposta (`201 Created`):**

```json
{
  "acknowledged": true,
  "insertedId": "6ae49ae84903bd8f7b574574"
}
```

- `acknowledged`: o MongoDB confirmou a gravação.
- `insertedId`: o `_id` do produto novo. Guarde esse valor para as outras rotas.

### 📋 Listar produtos

```bash
curl http://localhost:3000/produtos
```

**Resposta (`200 OK`):**

```json
[
  {
    "_id": "6ae49a807bdcc4f6c02a87f4",
    "nome": "Celular",
    "preco": 400,
    "categoria": "eletronico"
  },
  {
    "_id": "6ae49ac54903bd8f7b574573",
    "nome": "teclado",
    "preco": 400,
    "categoria": "periferico"
  },
  {
    "_id": "6ae49ae84903bd8f7b574574",
    "nome": "Mouse",
    "preco": 800,
    "categoria": "periferico"
  }
]
```

Se ainda não houver produtos, a resposta é uma lista vazia: `[]`.

### 🔍 Buscar produto por ID

```bash
curl http://localhost:3000/produtos/6ae49ae84903bd8f7b574574
```

**Resposta (`200 OK`):** o documento do produto. Se o ID tiver o formato certo mas não existir no banco, a resposta é `null`.

### ✏️ Atualizar produto

A atualização usa o operador `$set` do MongoDB. Isso significa que você envia **só os campos que quer mudar**, e os outros continuam como estavam.

```bash
curl -X PUT http://localhost:3000/produtos/6ae49ae84903bd8f7b574574 \
  -H "Content-Type: application/json" \
  -d '{"preco": 750}'
```

**Resposta (`200 OK`):**

```json
{
  "acknowledged": true,
  "modifiedCount": 1,
  "upsertedId": null,
  "upsertedCount": 0,
  "matchedCount": 1
}
```

- `matchedCount`: quantos documentos foram encontrados com aquele ID.
- `modifiedCount`: quantos realmente foram alterados.

### 🗑️ Excluir produto

```bash
curl -X DELETE http://localhost:3000/produtos/6ae49ae84903bd8f7b574574
```

**Resposta (`200 OK`):**

```json
{
  "acknowledged": true,
  "deletedCount": 1
}
```

Se `deletedCount` vier `0`, nenhum produto com aquele ID existia.

### 📡 Códigos de status HTTP usados

| Código | Significado | Quando aparece |
| ------ | ----------- | -------------- |
| `200 OK` | Deu certo | Listar, buscar, atualizar e excluir |
| `201 Created` | Recurso criado | Ao cadastrar um produto |
| `500 Internal Server Error` | Erro no servidor | Por exemplo, ao usar um ID com formato inválido |

---

## 🗂️ Estrutura do projeto

```
.
├── 📄 index.js            # Servidor Express, conexão com o MongoDB e todas as rotas
├── 📄 package.json        # Nome, dependências e configurações do projeto
└── 📄 package-lock.json   # Versões exatas das dependências instaladas
```

---

## 🔬 Como funciona por dentro

1. **Conexão:** o `index.js` cria um `MongoClient` com a `MONGO_URI` e pega a coleção `produtos` do banco `loja`. Se o banco ou a coleção ainda não existirem, o MongoDB os cria no primeiro cadastro.
2. **Leitura do corpo:** o `app.use(express.json())` faz o Express transformar o JSON enviado na requisição em um objeto JavaScript (`req.body`).
3. **Rotas:** cada rota chama uma operação do driver do MongoDB:

   | Rota | Operação no MongoDB |
   | ---- | ------------------- |
   | `POST /produtos` | `insertOne` |
   | `GET /produtos` | `find().toArray()` |
   | `GET /produtos/:id` | `findOne` |
   | `PUT /produtos/:id` | `updateOne` com `$set` |
   | `DELETE /produtos/:id` | `deleteOne` |

4. **ObjectId:** o ID chega na URL como texto, então o código usa `new ObjectId(req.params.id)` para converter no tipo que o MongoDB entende.
5. **Inicialização:** o servidor só começa a escutar na porta **3000** depois que a conexão com o banco é feita com sucesso. Se a conexão falhar, aparece `Erro ao conectar` com a mensagem do erro.
6. **async/await:** as operações no banco levam tempo, por isso as rotas são `async` e usam `await` para esperar a resposta do MongoDB antes de responder ao cliente.

---

## 🩺 Problemas comuns

| Sintoma | Causa provável | Como resolver |
| ------- | -------------- | ------------- |
| Erro logo ao iniciar, reclamando da connection string | A variável `MONGO_URI` não foi definida | Defina a variável no mesmo terminal onde roda `node index.js` |
| `Erro ao conectar` com mensagem de timeout ou de autenticação | String errada, senha incorreta ou IP não liberado no Atlas | Confira usuário e senha, e libere o seu IP em **Network Access** |
| `Cannot find module 'express'` | As dependências não foram instaladas | Rode `npm install` |
| Erro de sintaxe ou de versão ao iniciar | Node.js abaixo da versão exigida | Atualize para o Node 20.19.0 ou superior |
| `500` ao buscar, atualizar ou excluir | O ID não tem 24 caracteres hexadecimais | Copie o `_id` exato retornado pela API |
| `req.body` vazio ou `undefined` no teste | Faltou o cabeçalho `Content-Type: application/json` | Adicione o cabeçalho e envie o corpo como JSON |
| `EADDRINUSE` | A porta 3000 já está em uso | Feche o outro programa que usa a porta ou encerre o servidor anterior |

---

## 🖼️ Prints

### 🍃 Produtos salvos no MongoDB Atlas

Depois de cadastrar os produtos pela API, os documentos aparecem no **Data Explorer** do MongoDB Atlas, dentro do banco `loja`, na coleção `produtos`:

![Coleção produtos no MongoDB Atlas](./prints/mongodb-atlas-produtos.png)

O que dá pra ver nesse print:

- 🗄️ O caminho **Cluster0 → loja → produtos**, que é exatamente o banco e a coleção que o `index.js` usa
- 📄 Os **3 documentos** cadastrados (aba *Documents* com o número 3)
- 🔑 O `_id` de cada um, gerado automaticamente pelo MongoDB
- 🧾 Os campos `nome`, `preco` e `categoria`, enviados no corpo do `POST`

> 💡 **Atenção ao tipo do preço:** no print, o `preco` aparece entre aspas (`"400"`), ou seja, foi salvo como **texto**. Isso acontece quando o valor é enviado com aspas no JSON. Para guardar como **número** (melhor para ordenar e fazer contas), envie sem aspas: `"preco": 800`.
---

### 📖Get - Pegando o produto

![alt](image/image.png)

---
### 📨Post - Postando o produto

![alt](image/image-1.png)

---
### 📔Get - pegando pelo ID do produto


![alt](image/image-2.png)
---
### 🔄️Put - Atualizando o produto pelo ID

![alt](image/image-3.png)
---
### 🗑️Delete - Deletando o produto pelo ID


![alt](image/image-4.png)
---
### 📊MongoDB Atlas - finalização


![alt](image/image-5.png)
---

## 👨‍💻 Autor

**Caio Martins**
🎓 Estudante de Desenvolvimento de Sistemas no SENAI Americana (SP)
🔗 GitHub: [@caiomarcellino11](https://github.com/caiomarcellino11)