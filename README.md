<div align="center">

# 💹 Virtual Investment Advisor

**Full-stack robo-advisor: investor-profile suitability, Strategy-based recommended portfolios and a buy/sell marketplace on Oracle.**

![Node.js](https://img.shields.io/badge/Node.js_18+-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express_5-000000?style=for-the-badge&logo=express&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle_DB-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla_JS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)
![Jest](https://img.shields.io/badge/Jest-C21325?style=for-the-badge&logo=jest&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

**[🌐 Live demo](https://assessor-virtual-api.onrender.com)** · **[📖 API docs](https://assessor-virtual-api.onrender.com/api-docs)**

<img width="85%" alt="Virtual Investment Advisor dashboard" src="https://github.com/user-attachments/assets/958f11fe-bf72-42ef-b471-0525b9e5cdc2" />

</div>

---

## ✨ Overview

A simulated investment platform made of a **RESTful Node.js/Express API** backed by **Oracle Database** and a **vanilla JavaScript SPA**. Users sign up, verify their e-mail, answer a suitability questionnaire to get an investor profile (Conservative, Moderate or Aggressive), deposit funds, browse a marketplace of assets and build their own portfolio, or let the app invest everything in a recommended portfolio with one click.

> The free Render instance may take a few seconds to wake up on the first request.

## 🚀 Features

- 👤 **Accounts & security:** registration and login with bcrypt-hashed passwords, e-mail verification, forgot/reset password
- 🔐 **JWT access + refresh tokens**; verification, reset and refresh tokens are stored as **SHA-256 hashes** and compared in a timing-safe way, with a cron job that cleans expired tokens
- 🛂 **Role-based access** (`cliente` / `admin`) and owner-or-admin checks on portfolio routes
- 🧭 **Suitability questionnaire** that assigns the investor profile
- 🤖 **Recommended portfolio** per profile using the **Strategy pattern**, plus **one-click investing** of the whole balance
- 🛒 **Marketplace & portfolio:** deposit, buy by amount (the system computes fractional shares), sell by quantity
- 📊 **Dashboard** with live balance and a Chart.js allocation pie chart, light/dark theme
- 🧱 **Hardening:** `helmet` headers and `express-rate-limit` on the API and auth routes
- ✉️ **Transactional e-mails** through the Gmail API (Google OAuth2)
- 📖 **OpenAPI 3.0** spec with Swagger UI
- 🧪 **Jest + Supertest** integration and service tests

## 🧱 Tech Stack

| Layer | Technologies |
| --- | --- |
| Backend | Node.js, Express 5, `oracledb` connection pool, dotenv, node-cron |
| Security | jsonwebtoken, bcryptjs, helmet, express-rate-limit |
| E-mail | googleapis (Gmail API, OAuth2), Nodemailer MailComposer |
| Frontend | HTML5, CSS3, vanilla JS (ES modules, Fetch API), Chart.js |
| Docs & tests | swagger-ui-express, yamljs, Jest, Supertest |
| Database | Oracle Database |

## 🏛️ Architecture

Layered architecture applying SOLID principles:

```text
routes (src/api) ──▶ controllers ──▶ services ──▶ repositories ──▶ Oracle
        │                 │              │
   middlewares          DTOs        strategies (conservador · moderado · arrojado)
 (JWT, roles, rate limit)
```

- **Repository pattern** for data access, **DTOs** for validation between layers
- **Service layer** with injected repositories (dependency inversion)
- **Strategy pattern** for profile-specific portfolio recommendations
- **Middleware pattern** for authentication, authorization and rate limiting

<details>
<summary><b>📡 Main endpoints</b> (prefix <code>/api</code>)</summary>

| Area | Endpoints |
| --- | --- |
| Auth | `POST /auth/register` · `/auth/login` · `/auth/verify-email` · `/auth/forgot-password` · `/auth/reset-password` · `/auth/refresh-token` |
| Clients | `GET /clientes` · `GET/PUT/DELETE /clientes/:id` · `POST /clientes/:id/perfil` · `POST /clientes/:id/depositar` |
| Recommendations | `GET /clientes/:id/recomendacoes` · `POST /clientes/:id/recomendacoes/investir` |
| Portfolio | `GET /clientes/:id/carteira` · `POST /clientes/:id/carteira/comprar` · `POST /clientes/:id/carteira/vender` |
| Products (admin) | `GET/POST /investimentos` · `GET/PUT/DELETE /investimentos/:id` |

</details>

## ⚙️ Getting Started

**Requirements:** Node.js 18+ and access to an Oracle Database.

```bash
git clone https://github.com/pebarone/assessor-virtual-api.git
cd assessor-virtual-api
npm install
```

Create the schema and seed data by running [`migrations/create_tables.sql`](migrations/create_tables.sql) on your Oracle instance, then create a `.env` file:

```env
SERVER_PORT=3000
ENVIRONMENT=http://localhost:3000

DB_USER=your_oracle_user
DB_PASSWORD=your_oracle_password
DB_URL=host:1521/ORCL

SECRET=a_long_random_secret
REFRESH_SECRET=another_long_random_secret

EMAIL_USER=your_google_account
G_CLIENT_ID=...
G_CLIENT_SECRET=...
G_REDIRECT_URI=https://developers.google.com/oauthplayground
G_REFRESH_TOKEN=...

ADMIN_EMAIL=admin@admin.com
ADMIN_PASSWORD=admin
```

```bash
npm test     # Jest suite
npm start    # runs the tests first (prestart), then starts the server
```

Open `http://localhost:3000` for the app and `http://localhost:3000/api-docs` for Swagger.

**Docker image:**

```bash
docker pull docker.io/pbrnx/assessor-virtual-api:latest
docker run -p 3000:3000 --env-file .env pbrnx/assessor-virtual-api:latest
```

## 🗺️ Class Diagram

<img src="diagrama-classes.png" alt="Class diagram" width="85%">

## 🎓 Academic Context

Built as an academic project for the Software Engineering program at **[FIAP](https://www.fiap.com.br/)**.

| Team member | RM |
| --- | --- |
| Pedro Augusto Carneiro Barone Bomfim | 99781 |
| João Pedro de Albuquerque Oliveira | 551579 |
| Matheus Augusto Santos Rego | 551466 |
| Ian Cancian Nachtergaele | 98387 |
