# Email testing

Use this guide to test outgoing email locally. It explains:

- How testing works: the app still hands messages to SMTP, but you point it at a local inbox instead of a real provider on the public internet.
- What has to happen for messages to be produced, and how to create that activity for testing.
- How to open and inspect messages in a testing tool like Mailpit (subject, body text, layout, links).

For how digests choose content, schedules, and where the code lives, see [Digest emails](../features/digest-emails.md).

## Local inbox

For local verification, Mailpit is a simple and safe option: it runs an SMTP server on your machine and shows messages in a browser UI. Nothing is delivered to real inboxes on the internet. For more about Mailpit, see [https://mailpit.axllent.org/](https://mailpit.axllent.org/).

Follow the steps in order: Mailpit must be listening before the backend sends mail; the backend must read your updated mail settings before a test run.

**1. Start Mailpit**

```bash
docker run -d --name forus-mailpit -p 1025:1025 -p 8025:8025 axllent/mailpit
```

**2. Point the backend at Mailpit** in `backend/.env`:

```env
MAIL_DRIVER=smtp
MAIL_HOST=127.0.0.1
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

**3. Pick up `.env` changes** — PHP loads environment values when the process starts. After saving `backend/.env`, restart whatever runs Laravel for you (for example the Docker Compose PHP/backend service, or `php artisan serve` if you use that). One-off `php artisan …` commands start a new process each time and usually see the new values. If mail settings still look stale, run `php artisan config:clear` (in case `config:cache` was used earlier).

**4. Open the inbox** when you want to read messages: `http://127.0.0.1:8025`

## Quick smoke test to check if it works

From `backend/`, send a simple email through Laravel:

```bash
php artisan tinker --execute='Illuminate\Support\Facades\Mail::raw("Mailpit test from Forus local", function ($m) { $m->to("local-test@example.com")->subject("Forus Mailpit test"); });'
```

Refresh Mailpit and confirm the message appears.

## Digest email testing

Digest commands only send mail when they find **new** matching activity since each organization’s or fund’s last digest time (see [Digest emails](../features/digest-emails.md)). Running `test-data:seed` fills the database and disables outgoing mail during the seed run; it does not create ongoing digest-visible activity by itself.

**Prerequisites:** complete [Local setup](../local-setup.md) and [Seeding test data](../seeding-test-data.md) once so baseline organizations, funds, and identities exist.

**Typical flow:**

1. Configure Mailpit and `backend/.env` as in [Local inbox](#local-inbox).
2. Use the dashboards (sponsor, provider, requester, validator) to perform real actions that produce the kinds of events described in [Digest emails](../features/digest-emails.md): approvals, reservations, fund requests, provider messages, product updates where applicable, and so on. Repeat or combine actions until you expect a digest run to include something new.
3. From `backend/`, run the digest Artisan commands you want to inspect (for example `php artisan forus.digest:all`, plus any separate commands mentioned in `backend/app/Console/Commands/Digests/` or your scheduler setup).
4. Inspect messages in Mailpit.

Digest behaviour depends on selection logic and timestamps; recipient counts and subjects vary with data and locale.

### Singular and plural wording

Many digest strings use Laravel `trans_choice(...)`. Where it applies, verify singular copy by ensuring only **one** new relevant item sits behind the digest window, and plural copy by creating **two or more** relevant items before running the digest command again.

### What to check in Mailpit

Use rendered emails for visual checks:

- subject and heading text;
- singular versus plural wording where `trans_choice` applies;
- general layout and readability;
- button labels;
- whether links render as buttons.

## Limitations

Local Mailpit testing does not replace production mail provider behaviour. Coverage depends on which flows you exercise manually; not every digest branch may be reachable without specific seeded data or UI paths.

Out of scope for this guide:

- every digest subsection and edge case;
- every reservation state and locale;
- automated assertions on rendered HTML;
- delivery beyond your local catcher.

## Troubleshooting

- **No email in Mailpit** — check `MAIL_HOST`, `MAIL_PORT`, and that Mailpit is running.
- **Digest command succeeds but sends no email** — there may be no fresh matching events since the last digest timestamp; perform new qualifying actions and retry.
- **Only one text variant appears** — adjust how many matching items you create before the digest run so singular versus plural paths can both appear.

## Related

- [Digest emails](../features/digest-emails.md)
- [Seeding test data](../seeding-test-data.md)
