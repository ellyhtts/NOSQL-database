# Atividade Prática – MongoDB: Antes e Depois

Este repositório contém a resolução de uma atividade prática de MongoDB, focada em operações de CRUD (Create, Read, Update, Delete), filtros e projeções.

## 🛠️ Configuração Inicial (Setup)

Criação do banco de dados, da coleção e inserção dos documentos iniciais:

```javascript
// Criar/Usar o banco de dados
use store

// Criar a coleção
db.createCollection("customers")

// Inserir os documentos na coleção
db.customers.insertMany([
  {
    "name": "Ana",
    "age": 25,
    "city": "Salvador",
    "active": true,
    "points": 120
  },
  {
    "name": "Bruno",
    "age": 32,
    "city": "Feira de Santana",
    "active": true,
    "points": 300
  },
  {
    "name": "Carlos",
    "age": 28,
    "city": "Salvador",
    "active": false,
    "points": 80
  },
  {
    "name": "Daniela",
    "age": 40,
    "city": "São Paulo",
    "active": true,
    "points": 500
  },
  {
    "name": "Eduarda",
    "age": 22,
    "city": "Rio de Janeiro",
    "active": false,
    "points": 50
  }
])
```

### Exercício 1 – Consulta
### Objetivo: Retornar apenas o nome e a cidade de Ana e Carlos.

```JavaScript
db.customers.find({ city: "Salvador" }, { _id: 0, name: 1, city: 1 })
```

### Exercício 2 – Atualização
### Objetivo: Alterar o status do Carlos para ativo (true).

```JavaScript
db.customers.updateOne({ name: "Carlos" }, {$set: { active: true } })
```
### Exercício 3 – Atualizar vários documentos
### Objetivo: Adicionar o campo "state": "BA" para todos os clientes de Salvador.

```JavaScript
db.customers.updateMany(
  { city: "Salvador" }, 
  { $set: { state: "BA" } }
)
```
### Exercício 4 – Incremento
### Objetivo: Aumentar a pontuação de Ana de 120 para 170.

 ```JavaScript
 db.customers.updateOne({ name: "Ana" }, { $inc: { points: 50 } })
 
```
### Exercício 5 – Inserção
### Objetivo: Adicionar um novo cliente (Fernando).

```JavaScript
db.customers.insertOne({
  name: "Fernando",
  age: 29,
  city: "Recife",
  active: true,
  points: 90
})
```

### Exercício 6 – Remoção
### Objetivo: Excluir a cliente Eduarda.

```JavaScript
db.customers.deleteOne({ name: "Eduarda" })
```

### Exercício 7 – Criar um novo campo
### Objetivo: Adicionar o campo vip: true para Daniela.

```JavaScript
db.customers.updateOne(
  { name: "Daniela" }, 
  { $set: { vip: true } }
)
```

### Exercício 8 – Remover um campo
### Objetivo: Remover o campo points do cliente Bruno.

```JavaScript
db.customers.updateOne(
  { name: "Bruno" }, 
  { $unset: { points: "" } }
)
```

### Exercício 9 – Ordenação
### Objetivo: Retornar os clientes ordenados por idade em ordem decrescente.

```JavaScript
db.customers.find().sort({ age: -1 })
```

### Exercício 10 – Filtro com múltiplas condições
### Objetivo: Retornar apenas o nome dos clientes ativos com mais de 30 anos.

```JavaScript
db.customers.find(
  { active: true, age: { $gt: 30 } }, 
  { _id: 0, name: 1 }
)
```

## Desafios
### Comandos para consultas extras sem alterar os documentos existentes:

### 1. Mostrar apenas os nomes dos clientes:

```JavaScript
db.customers.find({}, { _id: 0, name: 1 })
```

### 2. Contar quantos clientes existem:

```JavaScript
db.customers.countDocuments()

```
### 3. Contar apenas os clientes ativos:

```JavaScript
db.customers.countDocuments({ active: true })
```

### 4. Mostrar o cliente com maior pontuação:

```JavaScript
db.customers.find().sort({ points: -1 }).limit(1)
```

### 5. Mostrar o cliente com menor idade:

```JavaScript
db.customers.find().sort({ age: 1 }).limit(1)
```

### 6. Mostrar apenas clientes com pontuação entre 100 e 400:

```JavaScript
db.customers.find({ points: { $gte: 100,$lte: 400 } })
```

### 7. Mostrar apenas clientes das cidades de Salvador ou São Paulo:

```JavaScript
db.customers.find({ city: { $in: ["Salvador", "São Paulo"] } })
```

### 8. Mostrar todos os clientes ordenados por nome:

```JavaScript
db.customers.find().sort({ name: 1 })
```

### 9. Mostrar apenas os três primeiros clientes:

```JavaScript
db.customers.find().limit(3)
```

### 10. Mostrar apenas os clientes inativos:

```JavaScript
db.customers.find({ active: false })
```
