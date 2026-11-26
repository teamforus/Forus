# Local setup

How to run Forus on your own machine for research, development or testing.

## Overview

You can run Forus locally in two ways:

| Path | Best for |
| --- | --- |
| **Docker** (recommended if unsure) | Newcomers, or anyone who wants PHP, MySQL, and Apache managed for them |
| **Native** | Experienced developers who already run PHP, MySQL, and Node locally and want faster iteration |

- **Docker** — closer to production; versions are isolated; one stack starts the services (can feel slower on some machines).
- **Native** — you install and maintain PHP, MySQL, Node, and extensions yourself.

A web server (Apache / Nginx) on your host is **optional**. Docker includes one. Native setup can use `php artisan serve` instead.

### End-to-end flow

1. **Prepare your machine** — editor, terminal, Git, and (for Docker) Docker Desktop ([Before you start](#before-you-start)).
2. **Get the code** — clone the main repo **with submodules** ([Get the project](#get-the-project)).
3. **Start backend and frontend** — [Option A — Docker](#option-a--docker) or [Option B — Native](#option-b--native).
4. **Load sample data** — [seeding-test-data.md](seeding-test-data.md).
5. **Sign in and explore** — [login-and-test-users.md](login-and-test-users.md).

---

## Before you start

Install and check these **before** you run setup commands. Use the checklist that matches your path.

### Everyone

| Tool | Why you need it | Get it |
| --- | --- | --- |
| **Web browser** | Open `http://localhost:…` to verify the app | Any current Chrome, Firefox, Edge, or Safari |
| **Code editor** | Edit files; most editors include a terminal | [VS Code](https://code.visualstudio.com/), [Cursor](https://cursor.com/), or similar |
| **Terminal** | Run `git`, `docker`, and setup commands | macOS **Terminal** or Linux shell; on Windows we recommend **Git Bash** (see note below) |
| **Git** | Clone the repo and load **submodules** (`backend/`, `forus-frontend/`) | [git-scm.com/downloads](https://git-scm.com/downloads) — run `git --version` to confirm |
| **GitHub account** | Clone over HTTPS and open pull requests later | [github.com](https://github.com/) — optional only if someone else gave you a full copy of the project |

**Terminal on Windows:** We recommend **Git Bash**, which is installed with [Git for Windows](https://git-scm.com/downloads). It matches the bash-style commands in this guide (for example `./docker/cmd/start-docker-compose.sh`) and avoids many PowerShell quirks. In VS Code or Cursor you can set the default terminal profile to Git Bash. macOS and Linux can use the built-in terminal.

**Git is required** for the supported setup: the main Forus repo points at two submodule repositories. Cloning without submodules leaves `backend/` or `forus-frontend/` empty and later steps fail. Downloading a ZIP from GitHub is possible but easy to get wrong; use Git for your first setup.

### Docker path (Option A)

| Tool | Why you need it | Get it |
| --- | --- | --- |
| **Docker Desktop** | Runs PHP, MySQL, Apache, and Node **inside containers** — you do not install them on your host | [Docker Desktop](https://www.docker.com/products/docker-desktop/) |

Before running any `docker compose` command:

1. Install Docker Desktop.
2. **Open Docker Desktop** and wait until it reports that the engine is running.
3. In a terminal, run `docker --version` and `docker compose version`.

You do **not** need PHP, Composer, Node, or MySQL installed on your computer for the Docker path.

### Native path (Option B)

| Tool | Why you need it | Get it |
| --- | --- | --- |
| **PHP** + Laravel extensions | Run the backend | [PHP downloads](https://www.php.net/downloads) — see [Laravel server requirements](https://laravel.com/docs/11.x/deployment#server-requirements) |
| **Composer** | PHP dependencies | [getcomposer.org](https://getcomposer.org/) |
| **Node.js** (includes **npm**) | Frontend build and dev server | [nodejs.org](https://nodejs.org/) |
| **MySQL** (or compatible) | Database — configure in `backend/.env` | Your OS package manager or [MySQL downloads](https://dev.mysql.com/downloads/) |

### Not required for first run

- **GitHub CLI** (`gh`) — optional for pull requests from the command line.
- **phpMyAdmin** — included in the Docker backend stack at http://localhost:8080 (login `forus` / `forus`).

### First-time checklist (Docker)

Use this if you are new to the toolchain:

- [ ] Browser and code editor installed
- [ ] Terminal opens and `git --version` works
- [ ] Docker Desktop installed, **running**, and `docker compose version` works
- [ ] Project cloned with submodules; `backend/` contains files (not an empty folder)
- [ ] Backend steps completed; http://localhost:8000 responds
- [ ] Frontend steps completed; http://localhost:3000/webshop.general loads
- [ ] Continue with [seeding-test-data.md](seeding-test-data.md)

---

## Get the project

Clone the **main** Forus repository **with submodules** so `backend/` and `forus-frontend/` contain code:

```bash
git clone --recurse-submodules https://github.com/teamforus/Forus.git
cd Forus
```

If you already cloned without submodules (empty `backend/` or `forus-frontend/`):

```bash
git submodule update --init --recursive
```

Confirm the folders are populated, for example:

```bash
ls backend/composer.json forus-frontend/package.json
```

Both files should exist. Submodule layout is described in [CONTRIBUTING.md](../CONTRIBUTING.md).

---

## Option A — Docker

Detailed reference: [../backend/readme-docker.md](../backend/readme-docker.md) and [../forus-frontend/readme-docker.md](../forus-frontend/readme-docker.md).

### How editing works with Docker

You edit files **on your machine** in your editor. Containers mount the project folder, so:

- Code changes apply inside the container without copying files.
- Install, migrate, seed, and start commands run **inside** the container, for example `docker compose exec app bash -c "<command>"` (backend) or `sh -c` (frontend).
- Your host does not need PHP or Node when you use this path.

### Backend

Open a terminal, go to the `backend/` folder, then run:

```bash
cd backend
docker compose build
./docker/cmd/start-docker-compose.sh
docker compose exec app bash -c "php artisan db:seed"
docker compose exec app bash -c "php artisan test-data:seed"
```

- Backend API: http://localhost:8000
- phpMyAdmin (database UI): http://localhost:8080 — user `forus`, password `forus`

### Frontend

In a **second** terminal, go to `forus-frontend/`:

```bash
cd forus-frontend
cp env.example.js env.js
docker compose up -d
docker compose exec app sh -c "npm ci"
docker compose exec app sh -c "yes n | npm run start"
```

Open in your browser:

- Webshop: http://localhost:3000/webshop.general
- Sponsor dashboard: http://localhost:3000/dashboard.sponsor
- Provider dashboard: http://localhost:3000/dashboard.provider
- Validator dashboard: http://localhost:3000/dashboard.validator

### Verify (Docker)

- http://localhost:8000 — backend responds
- http://localhost:3000/webshop.general — webshop loads
- One dashboard URL above — admin UI loads

---

## Option B — Native

### Backend

From `backend/`:

```bash
cd backend
cp .env.example .env
composer install
php artisan key:generate
```

Configure your database in `.env`, then:

```bash
php artisan migrate
php artisan db:seed
php artisan serve
```

Default API URL: http://localhost:8000

### Frontend

From `forus-frontend/` (separate terminal):

```bash
cd forus-frontend
npm ci
cp env.example.js env.js
npm run start
```

Set `api_url` in `env.js` to your backend (for example `http://localhost:8000/api/v1`).

Open in your browser:

- Webshop: http://localhost:5000/webshop.general
- Sponsor dashboard: http://localhost:5000/dashboard.sponsor
- Provider dashboard: http://localhost:5000/dashboard.provider
- Validator dashboard: http://localhost:5000/dashboard.validator

### Verify (Native)

- http://localhost:8000 — backend responds
- http://localhost:5000/webshop.general — webshop loads
- One dashboard URL above — admin UI loads

---

## If you get stuck

| Symptom | What to check |
| --- | --- |
| `backend/` or `forus-frontend/` is empty | Run [Get the project](#get-the-project) submodule commands |
| `docker: command not found` | Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and use a new terminal |
| Cannot connect to the Docker daemon | Open Docker Desktop and wait until it is running |
| Command fails with “no such file” in `backend/` | Your shell is in the wrong folder — `cd` into `backend/` or `forus-frontend/` first |
| Frontend loads but API errors (native) | `env.js` → `api_url` must match your backend URL |
| Port already in use | Stop another app on port 8000, 3000, or 5000, or change ports in Docker/native config |

---

## Next

- [seeding-test-data.md](seeding-test-data.md) — sample organizations, funds, and test users
- [login-and-test-users.md](login-and-test-users.md) — sign in locally

## Related

- [testing/email.md](testing/email.md) — Mailpit and local mail testing
- [CONTRIBUTING.md](../CONTRIBUTING.md) — branches, commits, and pull requests
