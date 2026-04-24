# Seeding test data

Before using these commands, complete the basic local setup in [local-setup.md](local-setup.md). That setup prepares the backend, frontend, environment variables, and database connection.

To make the local environment usable for testing, the database needs data.

> **Important:** the seed is not designed to run multiple times. If data already exists it can fail or produce inconsistent results. If you need to seed again, reset the database first (see [Re-seeding](#re-seeding) below).

## What to run

### 1. Configure the primary email

Copy the custom config (only needed the first time):

```bash
cp -R ./config/forus/test_data/configs/custom.example ./config/forus/test_data/configs/custom
```

Then set `primary_email` in `./config/forus/test_data/configs/custom/config.php` to an email you can access (or any placeholder — see [login-and-test-users.md](login-and-test-users.md) for the dev token shortcut).

### 2. Reset the database (only if needed)

Skip this on a fresh setup. Only run it if the database already has data from an earlier run:

```bash
# Docker (from backend/):
docker compose exec app bash -c "php artisan migrate:fresh"

# Native (from backend/):
php artisan migrate:fresh
```

This drops all tables and re-runs the migrations.

### 3. Load the basic system data

```bash
# Docker (from backend/):
docker compose exec app bash -c "php artisan db:seed"

# Native (from backend/):
php artisan db:seed
```

This adds the shared data the application needs in order to run.

### 4. Load the test dataset

```bash
# Docker (from backend/):
docker compose exec app bash -c "php artisan test-data:seed"

# Native (from backend/):
php artisan test-data:seed
```

This adds practical test data (organizations, funds, products, and identities) so common user flows can be tested locally.

At the end, the command prints an **Access token**. Keep it — it is the fastest way to log in locally.

## Re-seeding

The seeders are **not idempotent**. Running them again on a populated database may fail or leave data in a mixed state. To re-seed cleanly:

1. Reset the database (step 2 above).
2. Re-run step 3 (`db:seed`) and step 4 (`test-data:seed`).

## Next

- Log in: [login-and-test-users.md](login-and-test-users.md)

## Related

- Setup: [local-setup.md](local-setup.md)
