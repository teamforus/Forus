# Digest emails

Digest emails summarize recent activity and notify users about actions they may need to take, such as validating new fund requests, reviewing provider applications, or accepting or rejecting product reservations. For people with vouchers, digest emails can highlight new spending options in the form of new providers or products available in the webshop. Instead of sending a separate email for every event, related updates are bundled into a single overview.

## How they work

The digest feature looks for relevant activity since the previous digest timestamp. The e-mails are built from event logs and sent by Artisan commands. If no new matching activity exists, the digest command will run successfully without sending any email. If it does find activity it will send the relevant e-mails.

As a side note in regard to first use after the initial local platform setup: The regular test-data seeder creates the baseline data: organizations, funds, providers, products, vouchers, reservations, and fund requests. Outgoing email is disabled while seeding, so those activities are out of scope of the first digest. This means that new activity within the system is needed to be able to trigger the digests.

## Relevant files and folders

- `backend/app/Digests/` — digest logic per type/audience (`provider_funds`, `requester`, `sponsor`, `validator`, etc.).
- `backend/app/Console/Commands/Digests/` — Artisan commands that trigger digest sending.
- `backend/resources/lang/nl/digests.php` — Dutch digest copy (subjects, headings, buttons, body text).
- `backend/resources/views/emails/mail-builder-template.blade.php` — shared HTML structure for digest rendering.
- `backend/config/forus/mail_styles.php` — shared mail styles (spacing, typography, buttons, separators).

## Where to change what

- To change digest texts, edit `backend/resources/lang/nl/digests.php`.
- To change digest event selection or recipient logic, edit the matching class in `backend/app/Digests/`.
- To change email layout/HTML structure, edit `backend/resources/views/emails/mail-builder-template.blade.php`.
- To change visual styling or spacing, edit `backend/config/forus/mail_styles.php`.
- To test output locally, follow `docs/testing/email.md`.

## Digest types

- `provider_funds` — provider fund application, approval, revocation, and sponsor feedback updates.
- `provider_products` — newly reserved products or services for a provider.
- `provider_reservations` — reservation status overview for provider products.
- `requester` — new providers or products available to a requester.
- `sponsor` — provider applications, product updates, provider messages, and related sponsor work.
- `sponsor_product_updates` — changed provider products that need sponsor attention.
- `validator` — new fund requests for validation.

## Triggers

All scheduled digest jobs run at 18:00. Most run daily; `provider_reservations` is the exception and runs weekly on Monday at 18:00. All digest jobs can be triggered manually with Artisan commands, for example when testing locally.

The requester digest exists as a command, but is not enabled in the default scheduler.

## Singular and plural text

Many digest strings use Laravel `trans_choice(...)`. This means the email can show different text for one item and multiple items. When changing digest copy, test both paths when possible:

- one new event to verify singular text;
- two or more new events to verify plural text.

## Testing

To visually inspect digest emails locally, you can use a tool like Mailpit and create fresh digest events after the baseline seed has run.

See [email testing](../testing/email.md) for the local testing flow.
