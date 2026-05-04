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

Digest commands only send mail when they find **new** matching activity since each organization’s or fund’s last digest time (see [Digest emails](../features/digest-emails.md)). Running `test-data:seed` fills the database and disables outgoing mail during the seed run; it does not replace the step of adding **fresh** digest-visible activity when you want to preview digests again.

You can create activity by using the product manually (reservations, approvals, and so on), but covering every digest path that way is slow and easy to miss. For local QA, use the `test-data:make-digest-events` command below: it appends digest-oriented EventLog rows on top of an already seeded database. The command does not cover every email path; see [Out of scope](#out-of-scope).

**Prerequisites:** complete [Local setup](../local-setup.md) and [Seeding test data](../seeding-test-data.md) once so baseline organizations, funds, and identities exist. You do not need to re-seed every time you test mail.

**Typical flow:**

1. Configure Mailpit and `backend/.env` as in [Local inbox](#local-inbox).
2. Create fresh digest test events: `php artisan test-data:make-digest-events` (see commands below).
3. Run the digest Artisan commands.
4. Inspect messages in Mailpit.

Create digest events in two modes:

```bash
php artisan test-data:make-digest-events --mode=singular
php artisan forus.digest:all
php artisan forus.digest:provider_reservations
php artisan forus.sponsor_products_update_digest

php artisan test-data:make-digest-events --mode=plural
php artisan forus.digest:all
php artisan forus.digest:provider_reservations
php artisan forus.sponsor_products_update_digest
```

Use `singular` to create one relevant event per digest type. Use `plural` to create two relevant events per digest type. This lets you verify both `trans_choice(...)` paths.

`forus.digest:all` does not include every digest command. Run `forus.digest:provider_reservations` and `forus.sponsor_products_update_digest` separately when testing all digest email templates.

## What to expect

The command appends activity on top of the existing seeded data. Digest emails are sent to all matching identities and organization employees, so each digest run typically produces several emails per type (one per recipient), not one email total.

Running each mode once on the default seeded dataset produces roughly **25 emails** in Mailpit, with this approximate distribution:

- ~21 requester webshop updates (one per matching identity-fund combination);
- 2 provider fund updates;
- 1 provider product reservation update;
- 1 provider reservations update;
- 1 sponsor product change update;
- 1 validator fund request update.

Plural mode produces roughly the same number of emails as singular; what changes is the wording inside each email (`trans_choice` singular versus plural strings), not the recipient count.

Use these emails for visual checks:

- subject and heading text;
- singular versus plural wording (compare a singular run with a plural run);
- general layout and readability;
- button labels;
- whether links render as buttons.

## What the command covers

The `test-data:make-digest-events` command appends EventLog rows for the following digest types:

- `provider_funds` — provider approval (budget) and an individual product approval.
- `provider_products` — product or service reservation events.
- `provider_reservations` — reservation status activity.
- `requester` — new providers approved for a fund.
- `sponsor` — provider feedback messages (when seeded chats exist).
- `sponsor_product_updates` — monitored product field changes (only when the sponsor has `allow_product_updates` enabled).
- `validator` — new fund requests.

## Out of scope

The `test-data:make-digest-events` command is for local visual QA. It does not exhaustively trigger every digest branch.

Out of scope:

- every provider fund sub-state (revoked, sponsor feedback, every individual product combination);
- sponsor digest subsections that need new `FundProvider` or `Product` rows (pending providers, approved providers, unsubscribed providers, new products);
- every reservation state;
- every locale;
- automated assertions on rendered HTML;
- real email delivery outside Mailpit.

## Troubleshooting

- **No email in Mailpit** — check `MAIL_HOST`, `MAIL_PORT`, and that Mailpit is running.
- **Digest command succeeds but sends no email** — there may be no fresh matching events since the last digest timestamp.
- **Only one text variant appears** — create both singular and plural digest events before running the matching digest commands.

## Related

- [Digest emails](../features/digest-emails.md)
- [Seeding test data](../seeding-test-data.md)
