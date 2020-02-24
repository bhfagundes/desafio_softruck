# Pasta destinada ao desenvolvimento do backend do desafio!

Foi utilizado como linguagem NodeJs e banco de dados MongoDB, utilizando TDD para desenvolvimento
Para realização do desafio foi utilizado o auxilio de alguns frameworks sendo eles:

- [Node.js](https://nodejs.org/en/) - Linguagem base utilizada de backend
- [Express](https://www.npmjs.com/package/express) - Framework node para auxiliar na criação de aplicação web
- [Mongoose](http://mongoosejs.com/) - Framework do mongoDB para auxiliar nas consultas de banco
- [Mongoose-Pagination](https://github.com/edwardhotchkiss/mongoose-paginate) - Framework para auxiliar na paginaçãod de resultados
- [Express-jsend](https://www.npmjs.com/package/express-jsend) - Framework para auxiliar no envio de mensagens
- [Express-validator](https://github.com/ctavan/express-validator) - Framework utilizado para validar campos
- [Bcrypt](https://github.com/dcodeIO/bcrypt.js) - Framework usado para criptografar senha
- [Jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) - Framework usado para gerar um token visando facilitar o trafego
- [Mocha](https://www.npmjs.com/package/mocha) - Framework para auxiliar nos testes assincronos
- [Chai](https://www.npmjs.com/package/chai) - Framework para auxiliar no desenvolvimento utilizando TDD
- [Sinon](https://www.npmjs.com/package/sinon) - Framework para auxiliar nos testes
- [Supertest](https://github.com/visionmedia/supertest) -Utilizado para testar API's
- [Eslint](https://www.npmjs.com/package/eslint) - Usado para garantir organização do código

# Descrição de arquitetura (inglês)

The app is designed to use a layered architecture. The architecture is heavily influenced by the Clean Architecture.[Clean Architecture](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html) is an architecture where:

1. **does not depend on the existence of some framework, database, external agency.**
2. **does not depend on UI**
3. **the business rules can be tested without the UI, database, web server, or any external element.**

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/719/1*ZNT5apOxDzGrTKUJQAIcvg.png" width="350"/>
  <img src="https://cdn-images-1.medium.com/max/900/0*R7uuhFwZbhcqZSvn" width="350" /> 
</p>

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/1200/0*rFs1UtU4sRns5vCJ.png" width="350" />
  <img src="https://cdn-images-1.medium.com/max/1200/0*C-snK7L4sMn7b6CW.png" width="350" /> 
</p>

Also, in entry point(server.js), I use Dependency Injection(DI). There are many reasons using Dependency Injection as:

1. Decoupling
2. Easier unit testing
3. Faster development
4. Dependency injection is really helpful when it comes to testing. You can easily mock your modules' dependencies using this pattern.

You can take a look at this tutorial: `https://blog.risingstack.com/dependency-injection-in-node-js/`.
According to DI:
A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
B. Abstractions should not depend on details.

The code style being used is based on the airbnb js style guide.

## Data Layer

The data layer is implemented using repositories, that hide the underlying data sources (database, network, cache, etc), and provides an abstraction over them so other parts of the application that make use of the repositories, don't care about the origin of the data and are decoupled from the specific implementations used, like the Mongoose ORM that is used by this app. Furthermore, the repositories are responsible to map the entities they fetch from the data sources to the models used in the applications. This is important to enable the decoupling.

## Domain Layer

The domain layer is implemented using services. They depend on the repositories to get the app models and apply the business rules on them. They are not coupled to a specific database implementation and can be reused if we add more data sources to the app or even if we change the database for example from MongoDB to Couchbase Server.

## Routes/Controller Layer

This layer is being used in the express app and depends on the domain layer (services). Here we define the routes that can be called from outside. The services are always used as the last middleware on the routes and we must not rely on res.locals from express to get data from previous middlewares. That means that the middlewares registered before should not alter data being passed to the domain layer. They are only allowed to act upon the data without modification, like for example validating the data and skipping calling next().

## Entry point

Localizado no arquivo server.js.

# Iniciando o projeto

### Pré-requisito

É necessário criar o arquivo .env semelhane ao arquivo exemplo .env.example
Neste arquivo será setados informações relevantes

### Npm scripts:

```shell
npm run test
```

para executar testes.

# Endpoints da API

## Autenticação

### Registro

```shell
POST /auth/register
```

Body Parametros:

```shell
{
  name,
  surname,
  username,
  email,
  password
}
```

Descrição: Será criado um novo usuário e o password será salvo criptografado através do bcrypt.

### Login

```shell
POST /auth/login
```

Body Parametros:

```shell
{
  email,
  password
}
```

**Descrição**: Será retornado o token e algumas informações:

```js
{
    "status": "success",
    "data": {
        "token": {
          id: "eyJhbGciOiJIUzxxxxxxx.eyJlbWFpbCI6ImRpbW9zdGhlbxxxxxxxxxxxxx.axxxxxxxxxx",
          expiresIn: 86400,
        },
        "user": {
            "id": "mongoID",
            "fullName": "clark kent",
            "username": "superman",
            "email": "clarkkent@test.com",
            "created": "2018-01-08T14:43:32.480Z"
        }
    }
}
```

## Project Routes

Rotas para criar, editar, deletar e visualizar. (CRUD)

### Listagem de projetos

```shell
GET /project
```

**Descrição**: será retornada lista de tarefas para o usuário autenticado

```shell
POST /project
```

Body parametros:

```shell
{
  name, {String}
  status, {String}
  department, {String} (optional)
}
```

**Descrição**: criação de um novo projeto para o usuário autenticado.

```shell
GET /project/:projectId
```

```shell
DELETE /project
```

Body parametros:

```shell
{
  id, {String}

}
```

**Descrição**: Remove o projeto de id especificado

```shell
PATCH /project
```

Body parametros:

```shell
{
  id, {String}
  name, {String}
  status, {String}
  department, {String} (optional)
}
```

**Descrição**: Atualiza o projeto especificado.
