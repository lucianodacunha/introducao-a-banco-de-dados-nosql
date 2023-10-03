# Introdução a Banco de Dados NoSQL

**Vantagens / Desvantagens**
- Not Only SQL, pois não seguem modelo de tabelas relacionamentos.
- Projetado para lidar com alto volume de dados, flexibilidade.
- Não garantem consistência imediata.
- Menor suporte a consultas complexas.

**Principais Diferenças**

SQL | NoSQL |
---|---|
Modelo de dados fixo|Modelo de Dados Flexível|
Escalabilidade vertical|Escalabilidade horizontal|
Transações ACID 100%|Transações ACID ausentes total ou parcial|
Linguagem de consulta SQL|Cada SGBD tem sua própria|

**Visão geral do tipos de NoSQL**

- Tipos de DB: key-value, documentos, colunas, grafos, etc.

**Key-Value**

- par de chaves/valor único
- escalabilidade horizontal excelente
- simplicidade no armazenamento
- Redis, Riak, Amazon DynamoDb
- sessão de usuários.

**Document**

- documentos semiestruturados geralmente em formato JSON.
- MongoDb, CouchBase, Apache Couch
- Utilizado em catalogo de e-commerce.

**Coluna**

- utiliza o formato de coluna na armazenagem
- escalabilidade muito boa
- Apache Cassandra, Scylla DB, HBase 
- utilizado para logs

**Grafo**

- armazena e consulta dados que são interconectados.
- Neo4j, Amazon Neptune, JanusGraph
- armazena conexões e relacionamentos.

## Introdução ao MongoDB

**O que é**

- BD NoSQL
- Grandes volumes de dados, escalabilidade e flexibilidade
- Não exige esquema
- Armazena JSON

**Usos**

- Web, Big Data e aplicações de dados não estruturados, geolocalização, etc.

**Instalação e Configuração do MongoDB Atlas**

- [cloud.mongodb.com]()

**Modelagem de dados usando documentos**

- Estrutura
  - Database 
    - Coleções: Agrupamentos lógicos dos documentos e não exige estrutura.
      - Documentos: representação do dados a ser salvo
      - Documentos
      - Documentos

- Coleções: letra ou _ e até 64 bytes de comprimento.
- Documentos: 
  - JSON, semiestruturado, possui um identificador único.
  - máximo 16 Mb
  - Tipos: string, number, boolean, date, null e objectId
  - Outros: array, referencia, geojson e embedded doc.
- Estrutura:
  
```
{
    _id: ObjectId(""),
    "nome_campo": "Valor_campo",
}

```

- Modelagem de usuário e destino:

```
{

   "_id":1,

   "nome":"Usuario",

   "idade":24,

   "data_nascimento":"1990-01-01",

   "endereco":{

      "logradouro":"Rua Um, 2",

      "cidade":"Cidade"

   }

}

{

   "_id":1,

   "nome":"Praia",

   "cidade":"Litoral",

   "localizacao":{

      "type":"Point",

      "coordinates":[

         -46.0101,

         -21.0120

      ]

   }

}
```

**Estratégias de modelagem de dados eficientes e escaláveis**

- Deve ser orientada pelas consultas frequentemente realizadas.
- Inner documents: é comum desnormalizar os dados para evitar operações de joins custosas.
    - Os dados aninhados são:
      -  específicos do pai
      -  acessados juntamente com o documento pai
      -  cardinalidade um-para-muitos
- Referências: formas de relacionar documentos distintos.

**Operações no MongoDB**

```
use viagens;

db.createCollection("usuarios")
db.createCollection("destinos")


// Ou vc pode inserir diretamente um documento e ele já ira criar a collection
db.usuarios_novo.insertOne({});

// Inserindo o primeiro documento
db.usuarios.insertOne(
    {   
        "nome": "nome",
        "data_nascimento": "1990-10-05",
        "email": "pamela.apolinario.borges@gmail.com",
        "endereco": "Av Manoel Marques de Jesus, 380 - Vila Xavier, Araraquara/SP"
    });

db.usuarios.insertMany([
    {   
        "nome": "Pamela",
        "idade": 30,
        "email": "pamela.apolinario.borges@gmail.com",
        "endereco": "Av Manoel Marques de Jesus, 380 - Vila Xavier, Araraquara/SP"
    },
    {   
        "nome": "Pamela",
        "idade": 31,
        "email": "pamela.apolinario.borges.outra@gmail.com",
        "endereco": "Av Manoel Marques de Jesus, 380 - Vila Xavier, Araraquara/SP"
    },

]);

db.destinos.insertOne({"nome":"Praia do Rosa", "descricao":"LInda praia"})


//Inserindo mais usuarios

// Inserir documentos na coleção "usuarios"
db.usuarios.insertMany([{
    nome: "João",
    idade: 25,
    cidade: "São Paulo",
    estado: "SP",
    endereco: {
      rua: "Avenida Principal",
      numero: 123,
      cidade: "São Paulo",
      estado: "SP"
    }
  }, {
    nome: "Maria",
    idade: 30,
    cidade: "Rio de Janeiro",
    estado: "RJ",
    endereco: {
      rua: "Rua Secundária",
      numero: 456,
      cidade: "Rio de Janeiro",
      estado: "RJ"
    }
},{
    nome: "Carlos",
    idade: 20,
    cidade: "São Paulo",
    estado: "SP",
    endereco: {
      rua: "Rua Principal",
      numero: 789,
      cidade: "São Paulo",
      estado: "SP"
    }
  },{
    nome: "Ana",
    idade: 35,
    cidade: "São Paulo",
    estado: "SP",
    endereco: {
      rua: "Avenida Secundária",
      numero: 1011,
      cidade: "São Paulo",
      estado: "SP"
    }
    }
    ,  
    {
    nome: "Pedro",
    idade: 28,
    cidade: "Belo Horizonte",
    estado: "MG",
    endereco: {
      rua: "Rua Principal",
      numero: 1314,
      cidade: "Belo Horizonte",
      estado: "MG"
    }
  }]);
  

// Find
db.usuarios.find({});
db.usuarios.find({"nome": "João"});
db.usuarios.findOne({"nome": "João"});
db.usuarios.findOneAndUpdate({ nome: "João" }, { $set: { idade: 26 } });
db.usuarios.findOneAndDelete({ nome: "João" });

// Update
db.usuarios.updateOne(
  { nome: "João" },
  { $set: { idade: 26 } }
);

db.usuarios.updateMany(
  { cidade: "São Paulo" },
  { $set: { estado: "SP" } }
);
db.usuarios.replaceOne(
  { nome: "João" },
  {
    nome: "John",
    idade: 27,
    cidade: "São Paulo",
    estado: "SP",
    endereco: {
      rua: "Avenida Principal",
      numero: 123
    }
  }
);

// Update Operadores
// Usando o operador $set para definir o valor de um campo específico
db.usuarios.updateOne({ nome: "João" }, { $set: { idade: 26 } });

// Usando o operador $inc para incrementar o valor de um campo numérico
db.usuarios.updateOne({ nome: "João" }, { $inc: { idade: 1 } });

// Usando o operador $rename para renomear um campo existente
db.usuarios.updateOne({ nome: "João" }, { $rename: { "endereco.rua": "endereco.nomeRua" } });

// Usando o operador $unset para remover um campo específico de um documento
db.usuarios.updateOne({ nome: "João" }, { $unset: { endereco: "" } });

// Delete
// Usando o método deleteOne() para excluir o primeiro documento que corresponde ao filtro especificado
db.usuarios.deleteOne({ nome: "João" });

// Usando o método deleteMany() para excluir todos os documentos que correspondem ao filtro especificado
db.usuarios.deleteMany({ cidade: "São Paulo" });


// Operadores Lógicos
db.usuarios.find({ $and: [{ idade: { $gte: 18 } }, { cidade: "São Paulo" }] });

db.usuarios.find({ $or: [{ idade: { $lt: 18 } }, { cidade: "Rio de Janeiro" }] });

db.usuarios.find({ idade: { $not: { $eq: 25 } } });

// Operadores de Comparação
db.usuarios.find({ idade: { $eq: 25 } });

db.usuarios.find({ idade: { $ne: 30 } });

db.usuarios.find({ idade: { $gt: 30 } });

db.usuarios.find({ idade: { $gte: 30 } });

db.usuarios.find({ idade: { $lt: 30 } });

db.usuarios.find({ idade: { $lte: 30 } });

db.usuarios.find({ cidade: { $in: ["São Paulo", "Rio de Janeiro"] } });

db.usuarios.find({ cidade: { $nin: ["São Paulo", "Rio de Janeiro"] } });


// Projeção
db.usuarios.find({}, { nome: 1, idade: 1 })

// Ordenação
db.usuarios.find().sort({ idade: 1 })
// Limitação
db.usuarios.find().limit(10)
// Paginação
db.usuarios.find().skip(10).limit(5)
```

**Consulta simples**

- Operadores lógicos: $and, $or, $not
- Operadores de comparação: $eq, $ne, $gt, $gte, $lt, $lte, $in, $nin
- Projeções: definir quais campos devem ser retornados em uma consulta.
- Ordenação: ordenação de campos, asc, desc, etc.
- Limitação: quantidade de registros retornados.
- Paginação: limite cada grupo de informações retornadas.
  - ```db.usuarios.find().skip(10).limit(5)```


## Breve apresentação do Redis

- armazena dados em memoria.
- estrutura versatil
- operacoes atomicas
- cache de alto desempenho
- pub/sub
- contagem de acesso
- gerenciamento de sessoes
- cache de consultas
- comandos: set, get, del, exists, keys, incr, decr.
- https://try.redis.io/


## Para saber mais

- [jsonformatter](jsonformatter.curiousconcept.com)
- https://try.redis.io/
- [Slides](https://academiapme-my.sharepoint.com/:p:/g/personal/renato_dio_me/ESltHq--ANNPipSaRq3Z7TcBVibH3XKEnwHAkC-tS_-DUw?rtime=62Z2R13E20g)