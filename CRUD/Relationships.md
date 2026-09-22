# MongoDB - Relacionamentos 

Anotações simples sobre como relacionar dados no MongoDB.

---

## Primeiro: as 2 formas de relacionar dados

No MongoDB não existe um jeito "certo" fixo. Você escolhe entre:

- **Embutido (Embedded):** você coloca um dado *dentro* do outro. Tipo uma "caixinha dentro da caixinha".
- **Referência (Referenced):** você separa os dados em coleções diferentes e liga eles com um ID (tipo uma "chave estrangeira" do SQL, só que se chama `ObjectId`).

**Pensa assim:** se os dados sempre são usados juntos → embute. Se eles crescem muito ou vivem "sozinhos" → referencia.

---

## 1️. Um pra Um (1:1)

Exemplo: uma pessoa tem um carro só.

### Jeito embutido
Coloca tudo no mesmo documento. Bom quando o dado é pequeno e sempre anda junto.

```javascript
db.patients.insertOne({
  name: "lulu",
  age: 25,
  diseaseSummary: {
    diseases: ["cold", "broken leg"]
  }
})
```
Aqui o histórico médico tá "dentro" do paciente. Simples assim.

### Jeito com referência
Separa em duas coleções e liga com o ID.

```javascript
// Cria a pessoa
db.persons.insertOne({
  name: "Ray",
  age: 56,
  salary: 3000
})

// Cria o carro apontando pro dono
db.cars.insertOne({
  model: "BMW",
  price: 40000,
  owner: ObjectId("6aa9e2cee9c288ce1241317e")
})
```
Repara: o carro guarda o `ObjectId` da pessoa. É tipo um "link" entre os dois.

---

## 2️. Um pra Muitos (1:N)

Exemplo: uma pergunta pode ter várias respostas.

### Jeito embutido (quando é pouca coisa)
Se a lista não vai crescer muito (tipo, algumas respostas em um fórum), pode colocar tudo junto num array.

```javascript
db.questionThreads.insertOne({
  creator: "Mila",
  question: "How does that work?",
  answers: [
    { text: "Like that." },
    { text: "Thanks!" }
  ]
})
```

### Jeito com referência (quando é MUITA coisa)
Uma cidade pode ter milhões de moradores. Se tentar colocar todo mundo dentro de um array só, o documento estoura (o MongoDB tem limite de **16MB por documento**). Aí é melhor separar.

```javascript
// Cria a cidade
db.cities.insertOne({
  name: "New York City",
  coordinates: { lat: 21, lng: 55 }
})

// Cada cidadão aponta pra cidade dele
db.citizens.insertMany([
  { name: "Mila", cityId: ObjectId("5b98d6b44d01c52e1637a99f") },
  { name: "Brenno", cityId: ObjectId("5b98d6b44d01c52e1637a99f") }
])
```

> **Regrinha:** lista que pode crescer sem parar = usa referência.

---

## 3️. Muitos pra Muitos (N:N)

Exemplo: um livro pode ter vários autores, e um autor pode escrever vários livros.

### Jeito embutido (pra guardar uma "foto" do momento)
Aqui a ideia é guardar como as coisas estavam *naquele momento* da compra, mesmo que o preço mude depois.

```javascript
db.customers.insertOne({
  name: "Elly",
  age: 20
})

db.customers.updateOne(
  { name: "Elly" },
  {
    $set: {
      orders: [
        { title: "A Book", price: 12.99, quantity: 2 }
      ]
    }
  }
)
```
Se o preço do livro mudar amanhã, o pedido antigo continua com o preço de quando foi comprado. Isso é bom pra histórico de compras!

### Jeito com referência (pra não duplicar dado)
Se colocasse os dados completos dos autores dentro de cada livro, ia duplicar informação toda hora. Melhor só guardar os IDs.

```javascript
db.authors.insertMany([
  { name: "Jorge Amado", age: 78, address: { street: "Bahia" } },
  { name: "Graciliano Ramos", age: 55, address: { street: "Rio de Janeiro" } }
])

db.books.updateOne(
  { title: "Livro Exemplo" },
  {
    $set: {
      authors: [
        ObjectId("5b98d9e44d01c52e1637a9a6"),
        ObjectId("5b98d9e44d01c52e1637a9a7")
      ]
    }
  }
)
```

---

## Resumindo tudo numa tabela

| Relação | Embutido quando... | Referência quando... |
|---|---|---|
| 1:1 | Dado pequeno, sempre usado junto | Dado grande, usado separado |
| 1:N | Lista pequena/limitada | Lista que cresce infinito |
| N:N | Quero guardar um "histórico fixo" | Quero evitar dado duplicado |

---

## A regra de ouro 

> **Dado que é usado junto, deve ficar guardado junto.**

Ou seja: na dúvida, **embuta**. Só separa em referência se tiver um motivo real, tipo:

- Vai passar de 16MB no documento
- O dado se repete em vários lugares e você teria que atualizar tudo toda vez
- Você precisa consultar aquele dado sozinho, sem o resto

---
