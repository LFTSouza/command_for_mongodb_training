# :leaves: command_for_mongodb_training
Este repositório foi feito com o intuito de treinar querrys mais complexas no mongoshell. Utilizei algumas questões que foram produzidas pelo meu professor do instituto PROA e, também, utilizei uma empresa ficticia chamada "Momentos" também feitas pelo professor do instituto PROA.
## Questões e respostas:

- Quantos funcionarios da empresa Momento trabalham no departamento de vendas?
  
 > db.funcionarios.countDocuments({departamento:ObjectId("85992103f9b3e0b3b3c1fe71")})
  
- Inclua suas próprias informações no departamento de Tecnologia da empresa.

 > db.funcionario.insertOne({
  nome: "Luiz Felipe Tavares de Souza",
  telefone: "(99) 99999-9999",
  email: "luiz@momento.com",
  dataAdmissao: "2024-09-23",
  cargo: "Mobile Developer",
  salario: 3600,
  departamento: ObjectId("85992103f9b3e0b3b3c1fe74")
})


- Agora diga, quantos funcionários temos ao total na empresa?

> db.funcionarios.countDocuments();

- E quanto ao Departamento de Tecnologia?

> db.funcionarios.countDocuments({departamento: ObjectId("85992103f9b3e0b3b3c1fe74")})

- Qual a média salarial do departamento de tecnologia?

> db.funcionarios.aggregate([
  {
    $match:{departamento: ObjectId("85992103f9b3e0b3b3c1fe74")}
  },

  {
    $group: {
      _id: "$departamento",
      mediaSalario: {$avg: "$salario"}
    }
  }
])

- Quanto o departamento de Vendas gasta em salários?

> db.funcionarios.aggregate([
  {$match: {departamento : ObjectId("85992103f9b3e0b3b3c1fe71")}},
  {$group: {_id: "$departamento", somaSalario: {$sum: "$salario"}}}
])

- Um novo departamento foi criado. O departamento de Inovações. Ele será locado no Brasil. Por favor, adicione-o no banco de dados da empresa colocando quaisquer informações que você achar relevantes.

> db.escritrorios.insertOne(
  {
    nome: "Smzinho enterprises",
    endereco: "Smzinho enterprises, Ibura, Zona Norte, 425",
    telefone: "+55 81 9999-9999",
    pais: "Bra",
    suprimentos: [
      {
        produto: "Projetores",
        quantidade: 6,
        precoUnitario: 365.00
      },
      {
        produto: "Gorro_azul",
        quantidade: 3,
        precoUnitario: 16.90
      },
      {
        produto: "Teclado_Bt5.0",
        quantidade: 6,
        precoUnitario: 59.72
      },
    ]
  }
)

> db.departamentos.insertOne(
  {
    nome: "Inovações",
    escritorio: ObjectId("66f1e2042af2c812a3ac70a6")
  }
)

- O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.

> db.funcionarios.insertMany([
  {
    "nome": "Mariana",
    "telefone": "545.123.5123",
    "email": "Mariana@momento.com",
    "dataAdmissao": "2024-02-17",
    "cargo": "Pesquisadora",
    "salario": 2400,
    "departamento": ObjectId("66f1e2cd2af2c812a3ac70a7"),
  },
  {
    "nome": "Lucas Miranda",
    "telefone": "545.123.5321",
    "email": "Miranda@momento.com",
    "dataAdmissao": "2024-05-17",
    "cargo": "Pesquisador",
    "salario": 2400,
    "departamento": ObjectId("66f1e2cd2af2c812a3ac70a7"),
  },
  {
    "nome": "Matheus",
    "telefone": "545.123.5879",
    "email": "Matheus@momento.com",
    "dataAdmissao": "2024-02-17",
    "cargo": "Pesquisador",
    "salario": 2400,
    "departamento": ObjectId("66f1e2cd2af2c812a3ac70a7"),
  }
])

- Quantos funcionarios a empresa Momento tem agora?

>  db.funcionarios.countDocuments()

- Quantos funcionários da empresa Momento possuem conjuges?

> db.funcionarios.countDocuments({"dependentes.conjuge": {$exists: true}})

- Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO?

>   db.funcionarios.aggregate([
  {$match: {cargo: {$not: {$eq: "CEO"}}}},
  {$group: {_id: "$cargo", mediaSalario: {$avg: "$salario"}}}
])

- Qual a média salarial do departamento de tecnologia?

>  db.funcionarios.aggregate([
  {$match: {departamento: ObjectId("85992103f9b3e0b3b3c1fe74")}},
  {$group: {_id: "$departamento", mediaSalario: {$avg: "$salario"}}}
])

- Qual o departamento com a maior média salarial?

>  db.funcionarios.aggregate([
  {$group: {_id: "$departamento", mediaSalario: {$avg: "$salario"}}},
  {$sort: {mediaSalario: -1}},
  {$limit: 1}
])

- Qual o departamento com o menor número de funcionários?

> db.funcionarios.aggregate([
  {$group: {_id: "$departamento", qntdFuncionarios: {$sum: 1}}},
  {$sort: {qntdFuncionarios: 1}},
  {$limit: 1}
])

- Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?

> db.vendas.aggregate([
  {$group: {_id:"$produto", total: {$sum: {$multiply: ["$precoUnitario", "$quantidade"]}}}},
  {$sort: {total: -1}}, {$limit: 1}
])

- Qual o produto mais vendido da empresa?

> db.vendas.aggregate([
  {$group: {_id: "$produto", quantidade: {$sum: "$quantidade"}}},
  {$sort: {quantidade: -1}},
  {$limit: 1}
])

- Qual o produto menos vendido da empresa

>  db.vendas.aggregate([
  {$group: {_id: "$produto", quantidade: {$sum: "$quantidade"}}},
  {$sort: {quantidade: 1}},
  {$limit: 1}
])
