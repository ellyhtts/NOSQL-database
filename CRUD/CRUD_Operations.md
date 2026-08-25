# Operações CRUD

CRUD é um conjunto de operações básicas utilizadas para manipular dados em um banco de dados:

- **Create** → criar/inserir documentos.
- **Read** → consultar documentos.
- **Update** → atualizar documentos.
- **Delete** → excluir documentos.
  
## Operações CRUD — MongoDB
 
| Create                    | Read                    | Update                              | Delete                    |
|---------------------------|-------------------------|--------------------------------------|----------------------------|
| `insertOne(data, options)`  | `find(filter, options)`   | `updateOne(filter, data, options)`    | `deleteOne(filter, options)` |
| `insertMany(data, options)` | `findOne(filter, options)`| `updateMany(filter, data, options)`   | `deleteMany(filter, options)`|
|                            |                          | `replaceOne(filter, data, options)`   |                            |

# 1. Banco de dados
Neste exemplo será utilizado um banco de dados chamado `loja_informatica` e uma collection chamada `cliente`.

## Exibir os bancos de dados

O comando abaixo mostra os bancos de dados disponíveis no MongoDB:

```javascript
show databases
```

Também pode ser utilizado:

```javascript
show dbs
```

> **Observação:** um banco de dados só passa a aparecer na lista depois que algum dado é armazenado nele.

---

## Selecionar o banco de dados

Para trabalhar com um banco específico, utilizamos:

```javascript
use loja_informatica
```

Esse comando seleciona o banco `loja_informatica`.

> **Importante:** caso o banco ainda não exista, o MongoDB não o cria imediatamente. Ele será efetivamente criado quando algum documento for inserido.

---
# 2. Collection

## Criar uma nova collection

Para criar manualmente uma collection chamada `cliente`:

```javascript
db.createCollection("cliente")
```

A collection pode ser entendida como uma estrutura semelhante a uma tabela nos bancos relacionais, porém no MongoDB ela armazena **documentos no formato BSON**, semelhante ao JSON.

---

## Mostrar todas as collections

Para visualizar as collections existentes dentro do banco selecionado:

```javascript
show collections
```

O resultado deverá apresentar algo semelhante a:

```text
cliente
```

---

# CREATE — Inserção de documentos

A operação **Create** é responsável por inserir novos documentos na collection.

No MongoDB existem duas operações principais para inserção:

- `insertOne()` → insere um único documento.
- `insertMany()` → insere vários documentos de uma vez.

---

## 3. Inserir apenas um documento

Para inserir apenas um cliente:

```javascript
db.cliente.insertOne({
    "nome": "Amanda",
    "idade": 26,
    "pets": ["Gato", "Cachorro"],
    "endereco": {
        "logradouro": "Centro"
    }
})
```

### Explicação

O método `insertOne()` recebe um objeto e adiciona esse documento à collection.

Nesse exemplo, o documento possui:

- `nome` → nome do cliente.
- `idade` → idade do cliente.
- `pets` → uma lista contendo os animais de estimação.
- `endereco` → um objeto dentro do documento.
- `logradouro` → informação armazenada dentro de `endereco`.

O MongoDB também cria automaticamente um campo `_id` para identificar unicamente o documento.

---

## 4. Inserir vários documentos

Quando precisamos cadastrar vários clientes de uma vez, podemos utilizar `insertMany()`:

```javascript
db.cliente.insertMany([
    {
        "nome": "Brenno",
        "idade": 20
    },
    {
        "nome": "João",
        "idade": 25
    },
    {
        "nome": "Maria",
        "idade": 30
    },
    {
        "nome": "José",
        "idade": 28
    },
    {
        "nome": "Noé",
        "idade": 35
    }
])
```

O `insertMany()` recebe um **array de documentos** e realiza a inserção de vários registros em uma única operação.

---

# READ — Consulta de documentos

A operação **Read** é utilizada para consultar e visualizar os documentos armazenados.

As principais operações mostradas no exercício são:

- `find()` → encontra vários documentos.
- `findOne()` → encontra apenas um documento.

---

## 5. Mostrar todos os documentos

Para visualizar todos os clientes cadastrados:

```javascript
db.cliente.find()
```

O comando retorna todos os documentos existentes na collection `cliente`.

---

## 6. Buscar pelo campo

Podemos procurar documentos utilizando o valor de um determinado campo.

Por exemplo, para encontrar clientes cujo nome seja `"José"`:

```javascript
db.cliente.find({
    "nome": "José"
})
```

Nesse caso, o MongoDB procura todos os documentos que possuem:

```text
nome = José
```

---

## 7. Buscar apenas um documento

Quando queremos encontrar apenas um documento que corresponda ao filtro, utilizamos `findOne()`:

```javascript
db.cliente.findOne({
    "nome": "José"
})
```

A diferença principal é:

```javascript
db.cliente.find()
```

retorna **todos os documentos que correspondem ao filtro**.

Já:

```javascript
db.cliente.findOne()
```

retorna **apenas um documento**.

---

## 8. Buscar pelo identificador único

Todo documento criado pelo MongoDB recebe automaticamente um campo `_id`.

Esse identificador é único para cada documento.

Exemplo:

```javascript
db.cliente.find({
    "_id": ObjectId("6a7bbab007ff2cf8649f68a9")
})
```

Também podemos utilizar `findOne()`:

```javascript
db.cliente.findOne({
    "_id": ObjectId("6a7bbab007ff2cf8649f68a9")
})
```

> **Atenção:** o `ObjectId` precisa corresponder ao `_id` de um documento existente. O valor utilizado acima é apenas um exemplo.

---


