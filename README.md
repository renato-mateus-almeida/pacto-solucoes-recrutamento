# Pacto Solucoes — Recrutamento

Sistema de recrutamento com cadastro de vagas, candidaturas e avaliacoes.
Stack: **Spring Boot 4.1 (Java 21)** + **Angular 21** + **PostgreSQL 16**.

## Arquitetura

```
Browser (:8081) -> nginx (web) -> /api -> api (:8080) -> PostgreSQL (:5432)
```

## Pre-requisitos

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2.20+)
- [Git](https://git-scm.com/)

## Como rodar

### 1. Clone os repositorios

O projeto e composto por tres repositorios:

- **pacto-solucoes-recrutamento** — Repositorio principal (docker-compose, documentacao)
- **pacto-solucoes-recrutamento-api** — Backend em Spring Boot
- **pacto-solucoes-recrutamento-web** — Frontend em Angular

```bash
git clone https://github.com/renato-mateus-almeida/pacto-solucoes-recrutamento.git
cd pacto-solucoes-recrutamento
git clone https://github.com/renato-mateus-almeida/pacto-solucoes-recrutamento-api.git
git clone https://github.com/renato-mateus-almeida/pacto-solucoes-recrutamento-web.git
```

### 2. Suba os containers

```bash
docker compose up -d
```

Tres containers sobem automaticamente: `recrutamento-db`, `recrutamento-api` e `recrutamento-web`.

### 3. Acesse

[http://localhost:8081](http://localhost:8081)

### Credenciais

| Perfil | Email                         | Senha       |
|--------|-------------------------------|-------------|
| Admin  | `admin@pactosolucoes.com`     | `admin123`  |

Para criar um usuario comum, use a tela de registro no proprio sistema ou a rota `POST /api/v1/auth/register`.

### Variavel de ambiente

| Variavel        | Obrigatoria | Padrao                                                  |
|-----------------|-------------|----------------------------------------------------------|
| `APP_JWT_SECRET` | Nao         | Fallback definido no compose (minimo 32 caracteres)      |

Para producao, defina uma chave propria:

```bash
APP_JWT_SECRET=sua-chave-secreta-com-32-caracteres-minimo docker compose up -d
```

## Modo desenvolvimento

Caso prefira rodar as partes individualmente:

### Banco de dados

```bash
cd pacto-solucoes-recrutamento-api
docker compose up postgres -d
```

### Backend

```bash
cd pacto-solucoes-recrutamento-api
./mvnw spring-boot:run
```

API disponivel em `http://localhost:8080/api/v1`.

### Frontend

```bash
cd pacto-solucoes-recrutamento-web
npm install
npm start
```

Frontend disponivel em `http://localhost:4200`. As chamadas `/api` sao automaticamente redirecionadas para `localhost:8080` via proxy do Angular.

## API — Principais endpoints

| Metodo  | Rota                                 | Descricao              | Auth |
|---------|--------------------------------------|------------------------|------|
| `POST`  | `/api/v1/auth/register`             | Registrar usuario      | Nao  |
| `POST`  | `/api/v1/auth/login`                | Login (retorna JWT)    | Nao  |
| `GET`   | `/api/v1/vacancies`                 | Listar vagas           | Sim  |
| `GET`   | `/api/v1/vacancies/{id}`            | Detalhes da vaga       | Sim  |
| `POST`  | `/api/v1/vacancies`                 | Criar vaga (admin)     | Sim  |
| `POST`  | `/api/v1/vacancies/{id}/applications` | Candidatar-se        | Sim  |
| `GET`   | `/api/v1/dashboard`                 | Dashboard do candidato | Sim  |
