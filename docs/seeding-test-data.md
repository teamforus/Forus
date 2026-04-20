# Seeding test data

Before using these commands, complete the basic local setup in [local-setup.md](local-setup.md). That setup prepares the backend, frontend, environment variables, and database connection.

To make the local environment usable for testing, the database needs data. In most cases, you should first load the basic system data, then add the larger test dataset.

## What to run

Step 1, load the basic system data:

```bash
php artisan db:seed
```  
This adds the shared data the application needs in order to run.

Step 2, load the test dataset:

```bash
# Docker (from backend/):
docker compose exec app bash -c "php artisan test-data:seed"

# Native (from backend/):
php artisan test-data:seed
```  
This adds practical test data (organizations, funds, products, and identities), so common user flows can be tested locally.

Before running it the first time, copy the custom config:

```bash
cp -R ./config/forus/test_data/configs/custom.example ./config/forus/test_data/configs/custom
```

Then set `primary_email` in `./config/forus/test_data/configs/custom/config.php` to an email you can access (or any placeholder - see [login-and-test-users.md](login-and-test-users.md) for the dev token shortcut).

At the end, the command prints an **Access token**. Keep it, because it is the fastest way to log in locally.

## Next

- Log in: [login-and-test-users.md](login-and-test-users.md)

## Related

- Setup: [local-setup.md](local-setup.md)
