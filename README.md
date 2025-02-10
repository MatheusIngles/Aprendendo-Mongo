# Aprendendo Monngo
# Cola Banco de Dados MongoDB

## Comandos Básicos

- `show dbs` - Mostra os bancos de dados.
- `use <database>` - Entra em um banco de dados.
  - Caso o banco de dados não exista, ele será criado automaticamente, mas só aparecerá na listagem após conter uma coleção.

## Sintaxe de Funções

- `db.createCollection("<nome>")` - Cria uma coleção.
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
- Exemplo:
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

### Ordenação e Limite

- `db.students.find().sort({ name: 1 | -1 })` - Ordena alfabeticamente (`1`) ou em ordem reversa (`-1`).
- `db.students.find().limit(N)` - Limita a quantidade de documentos retornados.

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

