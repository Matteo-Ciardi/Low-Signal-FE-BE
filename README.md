<a id="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">

  <a href="https://lowsignal.vercel.app/">
    <img src="img/favicon.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">Low Signal</h3>

  <p align="center">
    E-commerce streetwear diviso in due repository: frontend React e backend Spring Boot, collegate come git submodule.
    <br />
    <br />
    <a href="https://lowsignal.vercel.app/">View Demo</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
        <li><a href="#frontend-low-signal">Frontend (low-signal)</a></li>
        <li><a href="#backend-low_signal">Backend (low_signal)</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## About The Project

<img src="img/Schermata 2026-11-27 alle 17.21.12.png" alt="demo-image">

Low Signal e' un e-commerce gestito da **due repository separate**, unite in questa cartella tramite **git submodule** (configurato in `.gitmodules`):

| Cartella      | Repository                                                                          | Ruolo                                                                           |
| ------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `low-signal/` | [Matteo-Ciardi/low-signal](https://github.com/Matteo-Ciardi/low-signal)             | **Frontend** — SPA React (Vite + Tailwind), deployata su Vercel                 |
| `low_signal/` | [Alessandro-Messineo/low_signal](https://github.com/Alessandro-Messineo/low_signal) | **Backend** — REST API Spring Boot, deployata su Render + Supabase (PostgreSQL) |

Come funziona il tutto:

- Il frontend chiama il backend tramite la variabile `VITE_API_URL` (axios, `withCredentials: true`).
- L'autenticazione usa **JWT in-memory** (access token) + **cookie di refresh** (sessione) con protezione **XSRF/CSRF**.
- I pagamenti passano da **Stripe** in modalita' **test** (il backend si rifiuta di partire con chiavi live).
- In produzione il frontend e' servito da Vercel e `vercel.json` inoltra `/api`, `/products`, `/orders`, `/payments`, `/newsletter`, `/users` al backend su Render.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

- [![React][React.js]][React-url]
- [![Vite][Vite.js]][Vite-url]
- [![Tailwind CSS][TailwindCSS.com]][Tailwind-url]
- [![Java][Java]][Java-url]
- [![Spring][Spring.io]][Spring-url]
- [![PostgreSQL][PostgreSQL]][PostgreSQL-url]
- [![Stripe][Stripe]][Stripe-url]
- [![Docker][Docker.com]][Docker-url]
- [![Vercel][Vercel]][Vercel-url]
- [![Render][Render]][Render-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

Per clonare il progetto completo e avviarlo in locale servono sia il frontend sia il backend.

### Prerequisites

- Git (con supporto ai submodule)
  ```sh
  git --version
  ```
- Node.js 20+ e npm
  ```sh
  node -v
  npm -v
  ```
- Java 21 (il backend e' Spring Boot 4 su Java 21)
  ```sh
  java -version
  ```
- Docker (opzionale, per avviare il backend in container)
  ```sh
  docker --version
  docker compose version
  ```
- Un database PostgreSQL (in produzione e' Supabase; in locale puoi usare un container Docker o un'istanza locale)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Installation

1. Clona il progetto **insieme ai submodule**:
   ```sh
   git clone --recurse-submodules https://github.com/Matteo-Ciardi/Low-Signal-FE-BE.git
   cd Low-Signal-FE-BE
   ```
2. Se hai gia' clonato senza `--recurse-submodules`, scarica le sottocartelle:
   ```sh
   git submodule update --init --recursive
   ```
3. Verifica che entrambe le cartelle siano popolate:
   ```sh
   git submodule status
   # low-signal  <hash> (...)
   # low_signal  <hash> (...)
   ```
4. Prosegui con le due sezioni seguenti ([Frontend](#frontend-low-signal) e [Backend](#backend-low_signal)).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Frontend (low-signal)

React 19 + Vite 8 + Tailwind CSS 4, con react-router, axios, Stripe Elements, react-hook-form + zod.

1. Entra nella cartella e installa le dipendenze:
   ```sh
   cd low-signal
   npm install
   ```
2. Crea un file `.env` nella root di `low-signal/`:

   | Variabile                     | Descrizione                                                                                                                                                                                               | Esempio                 |
   | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
   | `VITE_API_URL`                | URL base delle API backend. In produzione **deve** usare HTTPS (controllato a build time da `vite.config.js`). Se assente, le richieste vanno in same-origin e serve un reverse proxy che instradi `/api` | `http://localhost:8080` |
   | `VITE_STRIPE_PUBLISHABLE_KEY` | Chiave pubblicata Stripe (`pk_test_...`). Se assenta lo step di pagamento mostra "Configurazione mancante"                                                                                                | `pk_test_xxxxxxxxxxxx`  |

3. Avvia lo sviluppo:
   ```sh
   npm run dev        # server di sviluppo Vite
   npm run host       # idem, esposto sulla rete locale
   ```
4. Comandi utili:
   ```sh
   npm run build      # build di produzione in dist/
   npm run preview    # anteprima della build
   npm run lint       # ESLint
   npm run format     # Prettier su src/
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Backend (low_signal)

Spring Boot 4 (Java 21) + JPA/Hibernate + PostgreSQL + Spring Security (JWT, refresh cookie, CSRF) + Stripe.

1. Entra nella cartella:
   ```sh
   cd low_signal
   ```
2. Crea un file `.env` nella root di `low_signal/` (letto da `spring-dotenv` e da `docker-compose.yml`):

   | Variabile                | Descrizione                                                                      | Esempio                       |
   | ------------------------ | -------------------------------------------------------------------------------- | ----------------------------- |
   | `DB_HOST`                | Host del database PostgreSQL (Supabase in prod)                                  | `db.xxxxx.supabase.co`        |
   | `DB_PORT`                | Porta del database                                                               | `5432`                        |
   | `DB_NAME`                | Nome del database                                                                | `postgres`                    |
   | `DB_USERNAME`            | Utente del database                                                              | `postgres`                    |
   | `DB_PASSWORD`            | Password del database                                                            | `password`                    |
   | `JWT_SECRET`             | Secret usato per firmare gli access token                                        | `una-stringa-lunga-e-casuale` |
   | `JWT_EXPIRATION`         | Scadenza access token (ms)                                                       | `900000` (15 min)             |
   | `JWT_REFRESH_EXPIRATION` | Scadenza refresh token (ms)                                                      | `604800000` (7 giorni)        |
   | `FRONTEND_URL`           | Origin del frontend per il CORS (default `http://localhost:3000`)                | `http://localhost:5173`       |
   | `COOKIE_SECURE`          | `true` in produzione (HTTPS), `false` in locale                                  | `false`                       |
   | `COOKIE_SAME_SITE`       | Politica SameSite del cookie di refresh                                          | `Lax`                         |
   | `STRIPE_SECRET_KEY`      | Secret key Stripe — **deve** iniziare con `sk_test_`, altrimenti l'app non parte | `sk_test_xxxxxxxxxxxx`        |
   | `STRIPE_WEBHOOK_SECRET`  | Secret del webhook Stripe (`whsec_...`)                                          | `whsec_xxxxxxxxxxxx`          |
   | `STRIPE_CURRENCY`        | Valuta dei pagamenti                                                             | `eur`                         |
   | `STRIPE_MODE`            | Modalita' di pagamento — accettato **solo** `test`                               | `test`                        |
   | `PAYMENT_EXPIRY_MINUTES` | Minuti dopo i quali un ordine non pagato viene annullato                         | `15`                          |
   | `PAYMENT_EXPIRY_ENABLED` | Abilita' l'annullamento automatico                                               | `true`                        |
   | `PORT`                   | Porta HTTP (default `8080`; su Render e' assegnata dall'ambiente)                | `8080`                        |

3. Avvia il backend — due modi:

   **a. Con Maven (sviluppo in locale):**

   ```sh
   ./mvnw spring-boot:run        # Windows: .\mvnw.cmd spring-boot:run
   ```

   **b. Con Docker:**

   ```sh
   docker compose up --build      # legge le variabili da ./.env, espone la porta 8080
   ```

4. Esegui i test:
   ```sh
   test.bat                        # Windows: mvnw.cmd test con profile "test"
   ```
   oppure:
   ```sh
   ./mvnw test "-Dspring.profiles.active=test"
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

## Usage

### Avviare tutto in locale

1. Avvia il backend (porta **8080**):
   ```sh
   cd low_signal
   ./mvnw spring-boot:run
   ```
2. In un secondo terminale avvia il frontend (porta **5173** di default con Vite):
   ```sh
   cd low-signal
   npm run dev
   ```
3. Imposta nel `.env` del frontend `VITE_API_URL=http://localhost:8080` e nel `.env` del backend `FRONTEND_URL=http://localhost:5173` (la porta effettiva di Vite la vedi nel terminale), poi ricarica entrambi.

### Endpoints principali

| Endpoint                             | Descrizione                                    |
| ------------------------------------ | ---------------------------------------------- |
| `POST /api/auth/*`                   | Registrazione, login, refresh sessione, logout |
| `GET/POST /products`, `/collections` | Catalogo                                       |
| `POST /orders`, `GET /orders`        | Ordini                                         |
| `POST /payments/intent`              | Creazione PaymentIntent Stripe                 |
| `POST /payments/webhook`             | Webhook Stripe (richieste firmate)             |
| `POST /newsletter`                   | Iscrizione newsletter                          |
| `GET /health`                        | Health check                                   |

### Webhook Stripe in locale

Per ricevere gli eventi webhook in sviluppo usa il CLI di Stripe:

```sh
stripe listen --forward-to localhost:8080/payments/webhook
```

Copiata la `whsec_...` stampata dal CLI nel `.env` come `STRIPE_WEBHOOK_SECRET`.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->

## Contact

Matteo Ciardi - [@Matteo-Ciardi](https://github.com/Matteo-Ciardi) - ciardimatteo99@gmail.com

Alessandro Messineo - [@Alessandro-Messineo](https://github.com/Alessandro-Messineo) - alessandromessineo46@gmail.com

Project Link: [https://github.com/Matteo-Ciardi/Low-Signal-FE-BE](https://github.com/Matteo-Ciardi/Low-Signal-FE-BE)

Frontend: [https://github.com/Matteo-Ciardi/low-signal](https://github.com/Matteo-Ciardi/low-signal)

Backend: [https://github.com/Alessandro-Messineo/low_signal](https://github.com/Alessandro-Messineo/low_signal)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->

## Acknowledgments

<!-- - [Best-README-Template](https://github.com/othneildrew/Best-README-Template) -->

- [React Docs](https://react.dev/)
- [Vite Docs](https://vite.dev/guide/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Java Docs](https://docs.oracle.com/en/java/)
- [Spring Boot Docs](https://docs.spring.io/spring-boot/reference/)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [Stripe Docs](https://docs.stripe.com/)
- [Docker Docs](https://docs.docker.com/)
- [Vercel Docs](https://vercel.com/docs)
- [Render Docs](https://render.com/docs)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

[contributors-shield]: https://img.shields.io/github/contributors/Matteo-Ciardi/Low-Signal-FE-BE.svg?style=for-the-badge
[contributors-url]: https://github.com/Matteo-Ciardi/Low-Signal-FE-BE/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Matteo-Ciardi/Low-Signal-FE-BE.svg?style=for-the-badge
[forks-url]: https://github.com/Matteo-Ciardi/Low-Signal-FE-BE/network/members
[stars-shield]: https://img.shields.io/github/stars/Matteo-Ciardi/Low-Signal-FE-BE.svg?style=for-the-badge
[stars-url]: https://github.com/Matteo-Ciardi/Low-Signal-FE-BE/stargazers
[issues-shield]: https://img.shields.io/github/issues/Matteo-Ciardi/Low-Signal-FE-BE.svg?style=for-the-badge
[issues-url]: https://github.com/Matteo-Ciardi/Low-Signal-FE-BE/issues
[license-shield]: https://img.shields.io/github/license/Matteo-Ciardi/Low-Signal-FE-BE.svg?style=for-the-badge
[license-url]: https://github.com/Matteo-Ciardi/Low-Signal-FE-BE
[Next.js]: https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vite.js]: https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white
[Vite-url]: https://vitejs.dev/
[TailwindCSS.com]: https://img.shields.io/badge/Tailwind%20CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white
[Tailwind-url]: https://tailwindcss.com/
[Java]: https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white
[Java-url]: https://www.oracle.com/java/
[Spring.io]: https://img.shields.io/badge/Spring-6DB33F?style=for-the-badge&logo=spring&logoColor=white
[Spring-url]: https://spring.io/
[PostgreSQL]: https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white
[PostgreSQL-url]: https://www.postgresql.org/
[Stripe]: https://img.shields.io/badge/Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white
[Stripe-url]: https://stripe.com/docs
[Docker.com]: https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white
[Docker-url]: https://www.docker.com/
[Vercel]: https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white
[Vercel-url]: https://vercel.com/
[Render]: https://img.shields.io/badge/Render-46E3B7?style=for-the-badge&logo=render&logoColor=white
[Render-url]: https://render.com/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com/
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com/
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com/
