''''javascript'

// ==========================================
// PROJETO PRÁTICO: BANCO DE DADOS DE LIVRARIA
// ==========================================

// 1. Acessar ou criar o banco de dados da livraria

db.books.insertOne({
    "titulo": "Estruturas de Dados e Algoritmos",
    "autor": "Marcos Silva",
    "ano_publicacao": 2024,
    "categorias": ["Tecnologia", "Programação", "Backend"],
    "detalhes": {
        "paginas": 450,
        "idioma": "Português"
    },
    "estoque": 15,
    "preco": 89.90
})

// Inserindo múltiplos usuários na coleção "users"
db.users.insertMany([
    {
        "nome": "Amanda",
        "email": "amanda@email.com",
        "interesses": ["Java", "Python", "Banco de Dados"],
        "ativo": true
    },
    {
        "nome": "Carlos",
        "email": "carlos@email.com",
        "interesses": ["Design", "Figma"],
        "ativo": false
    }
])

// ------------------------------------------
// READ (Leitura e Consultas)
// ------------------------------------------

// Buscar todos os livros cadastrados
db.books.find()

// Buscar apenas os usuários que têm interesse em "Python"
db.users.find({ "interesses": "Python" })

// Buscar um livro específico pelo título
db.books.findOne({ "titulo": "Estruturas de Dados e Algoritmos" })

// ------------------------------------------
// UPDATE (Atualização de Dados)
// ------------------------------------------

// Atualizar o preço e o estoque de um livro específico utilizando o operador $set
db.books.updateOne(
    { "titulo": "Estruturas de Dados e Algoritmos" },
    { $set: { "preco": 79.90, "estoque": 10 } }
)

// Reativar a conta do usuário Carlos
db.users.updateOne(
    { "nome": "Carlos" },
    { $set: { "ativo": true } }
)

// ------------------------------------------
// DELETE (Exclusão de Registros)
// ------------------------------------------

// Remover um usuário específico pelo e-mail
db.users.deleteOne({ "email": "carlos@email.com" })

// Remover todos os livros que estão com o estoque zerado
db.books.deleteMany({ "estoque": 0 })

''''javascript'
