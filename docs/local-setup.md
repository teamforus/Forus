# Local setup

You can run Forus locally two ways. Pick one:

- **Docker** — closer to production, isolates PHP/MySQL/Apache versions, one command to start everything. Slower file I/O on some systems and a bit more overhead.
- **Native** — run PHP and Node directly on your machine. Fastest iteration, but you must manage PHP, MySQL, Node and extensions yourself.

If unsure, use Docker. If you already have PHP + MySQL + Node working locally, native is fine.

---

## Prerequisites

Both paths need:

- Git
- A MySQL-compatible database (bundled in Docker path)
- For native: PHP 8.x with Laravel's required extensions, Composer, Node 18+, npm.

---

## Option A — Docker

Full reference: [../backend/readme-docker.md](../backend/readme-docker.md) and [../forus-frontend-react/readme-docker.md](../forus-frontend-react/readme-docker.md).

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
- phpMyAdmin: http://localhost:8080 (`forus` / `forus`)

### Frontend

From `forus-frontend-react/`:

```bash
cp env.example.js env.js
docker compose up -d
docker compose exec app sh -c "npm i"
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
npm install
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
