# Teste Técnico - Backend Node.js com PokéAPI e PostgreSQL

Este teste técnico consiste no desenvolvimento de uma **API REST** em **Node.js** para integrar-se à [PokéAPI](https://pokeapi.co/) e armazenar dados em um banco de dados **PostgreSQL**. O objetivo é avaliar conhecimentos em:

- **Desenvolvimento de APIs REST** com **Node.js** e **Express**
- **Integração de API externa** utilizando **Axios**
- **Persistência de dados** em **PostgreSQL** usando **Prisma**
- **Paginação e filtros** em consultas de banco de dados
- **Boas práticas de estruturação de código e organização de projeto**
- **Tratamento de erros e respostas HTTP padronizadas**
- **Autenticação e autorização com JWT**
- **Testes automatizados (opcional, mas um diferencial)**

---

## 🎯 Requisitos

1. **Implementação da API REST** seguindo os princípios do RESTful.
2. **Integração com a PokéAPI** para buscar informações sobre Pokémons.
3. **Banco de Dados PostgreSQL** para armazenar os Pokémons cadastrados.
4. **Paginação e filtros** para a listagem de Pokémons.
5. **Boas práticas de desenvolvimento**, como separação de responsabilidades e organização modular.
6. **Uso de variáveis de ambiente** para armazenar credenciais sensíveis.
7. **Documentação básica** (README com instruções de instalação e uso da API).
8. **Utilização do Axios para realizar chamadas na PokéAPI e transformação dos dados para o formato solicitado da interface POKEMON.**
9. **Autenticação de usuários com JWT, exigindo login para acessar a listagem de Pokémons.**

---

## 🚀 Funcionalidades

### 📌 **1. Cadastro de Usuário**

- **Endpoint:** `POST /users/register`
- **Descrição:** Permite que um novo usuário se cadastre na aplicação.
- **Regras:**
  - O usuário deve fornecer um **e-mail** e uma **senha**.
  - A senha deve ser armazenada de forma segura utilizando **hashing**.
  - O e-mail deve ser único no sistema.

#### Corpo da requisição:
```json
{
  "email": "usuario@example.com",
  "password": "senhaSegura123"
}
```
#### Resposta de erro (401):

```json
{
  "error": "Usuário já cadastrado."
}
```
#### Resposta de erro (401):

```json
{
  "error": "E-mail ou senha inválidos."
}
```
#### Resposta de sucesso (201):
```json
{
  "message": "Usuário cadastrado com sucesso."
}
```

### 📌 **2. Login de Usuário**

- **Endpoint:** `POST /users/login`
- **Descrição:** Permite que um usuário faça login e obtenha um token JWT.
- **Regras:**
  - O usuário deve fornecer um **e-mail** e uma **senha**.
  - Se as credenciais forem válidas, um **token JWT** é retornado.

#### Corpo da requisição:
```json
{
  "email": "usuario@example.com",
  "password": "senhaSegura123"
}
```
#### Resposta de erro (401):

```json
{
  "error": "Credenciais inválidas."
}
```
#### Resposta de sucesso (200):
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### 📌 **3. Cadastrar um Pokémon**

- **Endpoint:** `POST /pokemons`
- **Descrição:** Recebe o nome de um Pokémon via corpo da requisição e consulta a PokéAPI para obter os dados do Pokémon. Em seguida, armazena os dados no banco de dados.
- **Regras:**
  - Não deve permitir a inserção de duplicatas.
  - Caso o Pokémon não seja encontrado na PokéAPI, deve retornar status **404 - Not Found**.
  - O objeto salvo deve conter: **id, nome, altura, peso, habilidades e imagem**.
  - A API deve utilizar **Axios** para realizar a chamada na **PokéAPI** e transformar os dados para o formato solicitado da interface **POKEMON**.
  - **Apenas usuários autenticados podem cadastrar Pokémons.**

  **POST** `/pokemons`  

#### Corpo da requisição:  

```json
{
  "name": "pikachu"
}
```

#### Resposta de sucesso (201):  

```json
{
  "id": 25,
  "name": "pikachu",
  "userId": 1,
  "height": 4,
  "weight": 60,
  "abilities": ["static", "lightning-rod"],
  "image": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/25.png",
  "createdAt": "2025-01-20T00:00:00.000Z",
  "updatedAt": "2025-01-20T00:00:00.000Z"
}
```
#### Resposta de erro (401):

```json
{
  "error": "Usuário não autenticado."
}
```
#### Resposta de erro (404):

```json
{
  "error": "Pokémon não encontrado."
}
```

### 📌 **4. Listar Pokémons**

- **Endpoint:** `GET /pokemons?page=1&limit=10&name=pikachu`
- **Descrição:** Retorna uma lista paginada de Pokémons registrados pelo usuário no banco de dados.
- **Regras:**
  - Deve permitir **paginação** (`page` e `limit`).
  - Deve permitir **filtro por nome**.
  - **Apenas usuários autenticados podem acessar essa rota.**

  #### Resposta:

```json
{
  "data": [
    {
      "id": 25,
      "name": "pikachu",
      "userId": 1,
      "height": 4,
      "weight": 60,
      "abilities": ["static", "lightning-rod"],
      "image": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/25.png",
      "createdAt": "2025-01-20T00:00:00.000Z",
      "updatedAt": "2025-01-20T00:00:00.000Z"
    }
  ],
  "total": 1,
  "currentPage": 1,
  "totalPages": 1
}
```
#### Resposta de erro (401):

```json
{
  "error": "Usuário não autenticado."
}
```

---

## 🏗️ Tecnologias Utilizadas

- **Node.js** (v14+)
- **Express** - Framework para API REST
- **Prisma** - ORM para PostgreSQL
- **PostgreSQL** - Banco de dados relacional
- **Axios** - Cliente HTTP para integração com a PokéAPI
- **dotenv** - Gerenciamento de variáveis de ambiente
- **jsonwebtoken (JWT)** - Autenticação e autorização
- **bcrypt** - Hashing seguro de senhas

---

## 📁 Estrutura do Projeto

Realize a estruturação do projeto da forma que preferir, mas se atente aos requisitos do teste e as boas práticas de desenvolvimento.
---

## ⭐ Diferenciais

- **Testes automatizados** com Jest ou outra biblioteca de testes.
- **Docker** para facilitar a execução do projeto.
- **Validações avançadas** (ex: Joi ou Zod).
- **Cache de respostas** para reduzir chamadas desnecessárias à PokéAPI.

---

## 📜 Licença

Este projeto é open-source e pode ser utilizado para fins educacionais e testes técnicos.

