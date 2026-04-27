# Login and test users (local dev)

Forus has **no fixed username/password**. Login is email-based (magic link / token).
Locally this can feel like "nothing happens" if mail is not configured, so this page
covers both the normal flow and a dev shortcut.

## How users normally log in

1. User enters their email on the sign-in page.
2. Frontend calls `POST /api/v1/identity/proxy/email` to request a login email.
3. User clicks the link in the email; the frontend exchanges the token for an access token.
4. Frontend stores the access token and loads the identity / organization data.

No password is ever asked.

## How to log in locally

### Preferred: normal email flow

Works best when your backend can actually send (or log) mail.

1. Seed base data and generate an identity:
   ```bash
   php artisan test-data:seed
   ```
2. Make sure mail works. Simplest local option — in `backend/.env`:
   ```
   MAIL_DRIVER=log
   ```
   Login links will be written to `backend/storage/logs/laravel.log` instead of being sent.
3. Go to a dashboard or webshop, enter the email you configured in
   `config/forus/test_data/configs/custom/config.php` (`primary_email`), and follow the link
   from the inbox or from the log file.

### Fast fallback: dev token shortcut

Use this when mail is broken or you just want to get in.

1. Run the seeder and copy the `Access token: ...` line it prints:
   ```bash
   php artisan test-data:seed
   ```
   Source: `backend/app/Console/Commands/Forus/TestDataSeedCommand.php:43`.
2. Open the frontend in your browser, open DevTools → Console, and run:
   ```js
   localStorage.setItem('active_account', 'PASTE_TOKEN_HERE');
   location.reload();
   ```
3. You should now be logged in as the seeded identity.

This bypasses email delivery entirely. Treat it as a dev-only convenience.

The token is tied to an identity, not to a specific dashboard, but the frontend must still send the right headers for its implementation. For the sponsor dashboard these are `Client-Key: general` and `Client-Type: sponsor` (set automatically by `env.js`). A wrong `Client-Key` returns `403` on `/api/v1/identity` and looks like a hang.

## Key considerations

- **Use normal email login for realistic testing.** The token shortcut skips parts of the auth flow and can hide bugs.
- **`client_key_api` / `Client-Key` header must match an implementation in the DB**, otherwise config endpoints return 403. This is usually set correctly by `env.js` and the seeder; only touch it if you know why.
- **Never set `active_account` to the string `"null"`.** It produces `Authorization: Bearer null` and every request 401s. If you hit that state, clear the key:
  ```js
  localStorage.removeItem('active_account');
  location.reload();
  ```
- **If the dashboard hangs after login** (requests pending, logo-only screen), the backend is stale. `php artisan serve` is single-threaded and keeps in-memory state from boot, so after a `test-data:seed` it can still point at rows the seeder replaced. Restart `php artisan serve` (or the backend container). Quick check from a terminal (replace `<TOKEN>`):
  ```bash
  curl -i --max-time 5 \
    -H 'Client-Key: general' -H 'Client-Type: sponsor' \
    -H 'Authorization: Bearer <TOKEN>' \
    http://127.0.0.1:8000/api/v1/identity
  ```
  Expect `HTTP 200`. If it hangs or returns `403`, restart `php artisan serve` (or the backend container) before anything else. No re-seed needed — your existing token stays valid.
- **Automated / headless browsers:** prefer the dev token shortcut over the email form. The email form submit relies on React state that synthetic typing does not always trigger, so the `POST /identity/proxy/email` request may never fire. Setting `localStorage.active_account` directly avoids this.

## Done

You are ready to test local user flows end to end.

## Related

- Setup guide: [local-setup.md](local-setup.md)
- Seed test data: [seeding-test-data.md](seeding-test-data.md)
