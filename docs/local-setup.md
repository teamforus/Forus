# Local setup

You can run Forus locally two ways. Pick one:

- **Docker** — closer to production, isolates PHP/MySQL/Apache versions, one command to start everything (can feel a bit slower on some systems).
- **Native** — run PHP and Node directly on your machine. You manage PHP, MySQL, Node and extensions yourself.

Recommendations:

- Use Docker if you are not an experienced developer, or if you are familiar with Docker and prefer it.
- Use native setup if you want finer control, faster iterations, or already manage PHP, MySQL and Node locally.

If unsure, use Docker.

A local webserver (Apache / Nginx) is **optional**. The Docker path includes one, and the examples below use it. The backend can also run without a webserver (for example via `php artisan serve`), so in a native setup you can skip it.

---

## Prerequisites

Both paths need:

- A MySQL-compatible database (bundled in the Docker path).
- For native: PHP with Laravel's required extensions, Composer, and Node.js (npm is included with Node.js).

Git is **optional**. You only need it if you want to clone the repository, switch branches, commit, or contribute. You can also download a branch directly from GitHub as a ZIP, but Git is easier.

---

## Option A — Docker

Full reference: [../backend/readme-docker.md](../backend/readme-docker.md) and [../forus-frontend-react/readme-docker.md](../forus-frontend-react/readme-docker.md).

### Editing files while using Docker

You edit files **locally on your machine** in your normal editor. The Docker containers mount the project directory, so:

- Code changes (backend PHP, frontend JS/TS, configs) take effect inside the container automatically; no copy step.
- Commands that install dependencies, run migrations, seed data, or start servers (`composer install`, `npm ci`, `php artisan migrate`, `npm run start`, etc.) run **inside the container**, via `docker compose exec app bash -c "<command>"` (or `sh -c` on the frontend).
- You do not need PHP or Node installed on your host when using Docker — the container has its own toolchain.

### Backend

From `backend/`:

```bash
docker compose build
./docker/cmd/start-docker-compose.sh
docker compose exec app bash -c "cp .env.example .env"
docker compose exec app bash -c "composer install"
docker compose exec app bash -c "php artisan key:generate"
docker compose exec app bash -c "php artisan migrate"
docker compose exec app bash -c "php artisan db:seed"
```

- Backend: http://localhost:8000
- phpMyAdmin (to inspect the database): http://localhost:8080 (`forus` / `forus`)

### Frontend

From `forus-frontend-react/`:

```bash
cp env.example.js env.js
docker compose up -d
docker compose exec app sh -c "npm ci"
docker compose exec app sh -c "yes n | npm run start"
```

Open:

- Webshop: http://localhost:3000/webshop.general
- Sponsor dashboard: http://localhost:3000/dashboard.sponsor
- Provider dashboard: http://localhost:3000/dashboard.provider
- Validator dashboard: http://localhost:3000/dashboard.validator

Verification checklist (Docker):
- Open http://localhost:8000 and confirm backend responds.
- Open http://localhost:3000/webshop.general and confirm the page loads.
- Open one dashboard URL and confirm it loads.

---

## Option B — Native

### Backend

From `backend/`:

```bash
cp .env.example .env
composer install
php artisan key:generate
php artisan migrate
php artisan db:seed
php artisan serve
```

By default this serves on http://localhost:8000.

Configure your database in `.env` before running `migrate`.

### Frontend

From `forus-frontend-react/`:

```bash
npm ci
cp env.example.js env.js
npm run start
```

Open:

- Webshop: http://localhost:5000/webshop.general
- Sponsor dashboard: http://localhost:5000/dashboard.sponsor
- Provider dashboard: http://localhost:5000/dashboard.provider
- Validator dashboard: http://localhost:5000/dashboard.validator

Make sure `env.js` points `api_url` at your running backend (e.g. `http://localhost:8000/api/v1` for the native path).

Verification checklist (Native):
- Open http://localhost:8000 and confirm backend responds.
- Open http://localhost:5000/webshop.general and confirm the page loads.
- Open one dashboard URL and confirm it loads.

## Next

- Seed test data: [seeding-test-data.md](seeding-test-data.md)

## Related

- Log in: [login-and-test-users.md](login-and-test-users.md)
