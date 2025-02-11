# Aprendendo Mongo
# Cola Banco de Dados MongoDB

## Comandos Básicos

- `show dbs` - Mostra os bancos de dados.
- `use <database>` - Entra em um banco de dados.
  - Caso o banco de dados não exista, ele será criado automaticamente, mas só aparecerá na listagem após conter uma coleção.

## Sintaxe de Funções

- `db.createCollection("<nome>", { capped: true, size: 1000000, max: 100 }, { autoIndexId: false })` - Cria uma coleção com limites opcionais.
- `show collections` - Lista as coleções do banco de dados.
- `db.dropDatabase()` - Exclui o banco de dados.
- `db.<coleção>.insertOne({})` - Insere um documento na coleção.
  - Se a coleção não existir, ela será criada automaticamente.
- `db.<coleção>.insertMany([{},{}])` - Insere múltiplos documentos em uma coleção.

## Atualizações

- `db.students.update({filtro}, {atualização})`
  - Semelhante ao `insert` em sintaxe, mas é recomendado atualizar com base no `_id`.
- `db.students.updateOne({},{$set:{} or $unset:{}})` - Atualiza um único documento.
- `db.students.updateMany({},{$set:{} or $unset:{}})` - Atualiza múltiplos documentos.

### Exemplo:
```js
 db.students.updateOne(
   { name: "Lucas", nota: null },
   { $set: { name: "Marcos", nota: 8.1 } }
 )
```

### Operadores Úteis:
- `{ $exists: false | true }` - Filtra documentos que possuem ou não um campo.
- `{ $ne: "" }` - Retorna documentos onde um campo **não seja** igual ao valor especificado.
- `{ $lt: valor }` - Menor que.
- `{ $lte: valor }` - Menor ou igual.
- `{ $gt: valor }` - Maior que.
- `{ $gte: valor }` - Maior ou igual.
- `{ $in: [valores] }` - Retorna documentos onde o campo tem um dos valores listados.
- `{ $nin: [valores] }` - Retorna documentos onde o campo **não** tem nenhum dos valores listados.

Exemplo:
```js
 db.students.updateMany(
   { nota: { $exists: false } },
   { $set: { nota: 1 } }
 )
```

## Remoção de Documentos

- `db.students.deleteOne({ filtro })` - Remove um único documento.
- `db.students.deleteMany({ filtro })` - Remove múltiplos documentos.

## Consultas (`find`)

- `db.<coleção>.find({},{})` - Equivalente ao `SELECT *` em SQL.
  - Pode receber `{query}, {projection}`.
- Exemplo de busca:
  ```js
  db.students.find({ nota: null })
  ```
- Busca com múltiplos critérios:
  ```js
  db.students.find({ nota: null, name: "Pedro" })
  ```
- Projeção (retorna apenas os campos especificados):
  ```js
  db.students.find({}, { name: 1 })
  ```

### Operadores Lógicos

- `{ $and: [{},{}] }` - Aplica uma condição **E**.
- `{ $or: [{},{}] }` - Aplica uma condição **OU**.
- `{ $nor: [{},{}] }` - Retorna documentos que **não** correspondem a nenhuma condição.
- `{ $not: {} }` - Equivalente ao operador **!** da programação.

### Ordenação e Limite

- `db.students.find().sort({ name: 1 | -1 })` - Ordena alfabeticamente (`1`) ou em ordem reversa (`-1`).
- `db.students.find().limit(N)` - Limita a quantidade de documentos retornados.

## Índices

- `db.students.createIndex({ name: 1 })` - Cria um índice para otimizar buscas.
- `db.students.getIndexes()` - Lista os índices existentes.
- `db.students.find({ name: "Lucas" }).explain("executionStats")` - Mostra informações sobre a performance da consulta.

Exemplo de saída de `getIndexes()`:
```js
[
  { v: 2, key: { _id: 1 }, name: "_id_" },
  { v: 2, key: { name: 1 }, name: "name_1" }
]
```

## Tipos de Dados

### Tipos Padrões
- **String**: `"Pedro123"` ou `'Pedro123'`
- **Integer**: `1234`
- **Double/Float**: `8.2`
- **Boolean**: `true` ou `false`
- **Null**: `null` (usado como placeholder)
- **Data**: `new Date("2023-01-02T00:00:00")`
  - Se não for declarado, assume a data e hora atuais.

## Relacionamentos e Estruturas de Dados

### Arrays (Lista de Dados)

Em um banco de dados relacional, geralmente utilizamos tabelas para representar relacionamentos `1-N`. No MongoDB, podemos armazenar múltiplos valores em um array:

```js
 db.funcionarios.insertOne({
   name: "Larry",
   idade: 12,
   graduacoes: ["Mestrado", "Pós-graduação"]
 })
```

### Documentos Aninhados (Nested Documents)

Em vez de criar múltiplas tabelas, podemos utilizar objetos aninhados para representar relações:

```js
 db.funcionarios.insertOne({
   name: "Larry",
   idade: 12,
   graduacoes: ["Mestrado", "Pós-graduação"],
   moradia: {
     rua: "Rua dos Loucos",
     numero: 7,
     cidade: "Vila Velha"
   }
 })
```


