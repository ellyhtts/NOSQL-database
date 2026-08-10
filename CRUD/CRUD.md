# Prática e Exercícios CRUD - MongoDB

Os comandos essenciais do shell exercícios práticos baseados nas operações fundamentais de banco de dados: CRUD (Create, Read, Update, Delete).

## Comandos Principais
Para manipular o banco de dados via terminal, utilizamos os seguintes comandos básicos:

* `mongosh`: Inicia o shell do MongoDB.
* `show databases` ou `show dbs`: Exibe todos os bancos de dados existentes no servidor.
* `use shop`: Seleciona ou cria implicitamente um banco de dados chamado "shop".
* `show collections`: Lista todas as coleções presentes no banco de dados atual.
* `db.createCollection("<collection_name>")`: Cria uma nova coleção de forma explícita.

---

## Exercícios Práticos (Operações CRUD)

As operações a seguir devem ser testadas no shell do MongoDB para consolidar a manipulação de dados.

### 1. Create (Criação)
A operação de criação permite inserir novos documentos em uma coleção, mesmo que ela tenha sido criada implicitamente.
* **Comando de Referência:** `insertOne(data, options)`.
* **Exercício:** Insira um novo registro executando o seguinte comando:
  `db.users.insertOne({ "name": "Ellen", "age": 20, "isStudent": true })`.

### 2. Read (Leitura)
A operação de leitura permite buscar e visualizar os documentos armazenados no banco.
* **Comandos de Referência:** `find(filter, options)` e `findOne(filter, options)`.
* **Exercício:** Liste todos os registros inseridos na coleção de usuários executando:
  `db.users.find()`

### 3. Update (Atualização)
A operação de atualização é responsável por modificar os dados de documentos já existentes na coleção.
* **Comandos de Referência:** `updateOne(filter, data, options)`, `updateMany(filter, data, options)` e `replaceOne(filter, data, options)`.
* **Exercício:** Utilize o comando `updateOne` para alterar a idade do usuário criado no passo 1.

### 4. Delete (Exclusão)
A operação de exclusão remove documentos específicos ou múltiplos documentos de uma coleção.
* **Comandos de Referência:** `deleteOne(filter, options)` e `deleteMany(filter, options)`.
* **Exercício:** Utilize o comando `deleteOne` passando um filtro (como o nome do usuário) para apagar o registro inserido no passo 1.
