# Glossary

Definitions for Forus domain terms used in contributor documentation. Terms are grouped by topic; **Note** lines give recommended wording where useful.

The platform and contributor documentation use English and are built to support any country and language. Forus began in the Netherlands; so some customer input and product labelling still reflects Dutch. You may see Dutch terms or Dutch-influenced English in the product and in older docs. This glossary stays in English and mentions Dutch only where it clarifies a term (for example *sociale regeling*, *Kindpakket*, *aanvrager*), not to document every locale.

## Core concepts

### Identity

A natural person in the system. The same identity can act as an applicant, as an employee of an organization, or in several roles at once. Permissions are tied to the identity.

In the database each identity has a numeric `id`, but the **stable system key** is **`address`**: an opaque token generated when the identity is created (`Identity::build`). Foreign keys and API auth use `address` (often named `identity_address` on related tables).

### Organization

A formal party in the system. It stands for a real-world organization such as a municipality, NGO, or charity (often as a **sponsor**), or a business that offers goods or services such as a shop, theatre, or swimming pool (often as a **provider**). Staff work through **identity** accounts linked to the organization as employees. One organization can hold one or more roles at the same time, for example sponsor, provider, and validator.

### Social benefit policy

The municipal or sponsor policy behind support offered through Forus (Dutch: *sociale regeling*; plural *sociale regelingen*). In the product, much of that policy is configured as a **fund**.

**Note:** Do not translate *regeling* / *regelingen* as “scheme”. In resident-facing text use **benefit** / **benefits**. In formal, municipal, policy, admin, or technical docs use **social benefit policy** / **social benefit policies**.

### Fund

A named **social benefit policy** that a **sponsor** organization runs in Forus (for example a child benefit fund, a city pass, or a swimming-lesson fund). The fund record holds the public name, description, start and end dates, and lifecycle state (`active`, `paused`, `closed`, or `waiting`).

Most behavior is configured on the linked **fund config**: who may apply (**criteria** and optional back-office checks), whether support is issued as **vouchers** or **payouts**, which flows are enabled (fund requests, prevalidation, direct requests, reservations, reimbursements, physical cards, and more), and which **providers** may take part. Amount rules live in fund formulas; budget is tracked through top-ups and issued vouchers. When the fund is public and tied to an **implementation**, residents see it on the municipal **website**; **webshop flows** apply only when that fund uses provider offers and related redemption. **Employees** manage the fund from the sponsor **admin panel**.

## Roles

### Sponsor

An organization role that creates and manages funds, eligibility, communications, and reporting. Municipalities and charities both use this role; the product stays generic.

**Note:** In contributor docs, **sponsor organization** is the usual term. In commercial conversations the same party is often called *client*; that still means an organization in the **sponsor** role here.

### Provider

An organization role that offers products or services and handles reservations and redemption flows. The same provider can be linked to funds from different **sponsor** organizations; residents need not see that network.

### Validator

An organization role that reviews fund requests before support is issued, when that flow is configured.


## Users

### User

Everyday word for a natural person that uses the system. It can mean **support recipient**, **employee**, or **identity**.

**Note:** In contributor docs, a specific role term is clearer.

### Citizen

Common municipal word for someone in the municipality. Not used the same way by charities; not a platform role name.

**Note:** On a municipality’s own **website** copy, *citizen* is common. In contributor docs, **support recipient** or **applicant** is usually clearer.

### Employee

An **identity** linked to a **sponsor**, **provider**, or **validator** organization through an `employees` record, with roles that allow work in the **admin panel** (for example fund manager, shop staff, validator). Not the same as someone applying for support on the public **website**.

**Note:** In contributor docs use **employee**, or **sponsor employee** / **provider employee** / **validator employee** when the organization type matters. Matches product labels such as *medewerker*.

### Applicant

An **identity** going through a **fund request** for a fund (approved, denied, or pending). Only applies while that application process is relevant.

**Note:** Matches UI terms such as *aanvrager*. After support is issued without a request, or when an **employee** issues it from the admin panel only, use **support recipient** instead.

### Requester

Product label used in digests and parts of the API (for example `RequesterDigest` for identities with certain **vouchers**). It does not cover everyone on the public **website** and not everyone who filed a **fund request**.

**Note:** In contributor docs, **applicant** fits **fund request** steps and **support recipient** fits issued support. Use **requester** when matching digest types, commands, or API names.

## Interfaces

Where residents and **employees** use the system.

### Website (public)

The resident-facing site for one municipality or brand: fund information, eligibility checks (for example regelingencheck), applications, CMS pages, sign-in, and—when the fund is set up for it—**webshop flows**.

**Note:** With clients, **website** or **municipal website** is usually clear. When there is no offer or redemption part, **website** fits better than **webshop** for the whole site.

### Implementation

Technical configuration for one public **website** plus linked **admin panels**: branding, URLs, CMS, pre-check, DigiD, mail settings, and which funds are attached (`implementations.key`, `Client-Key` header).

**Note:** With municipalities and customer-facing teams, **website** is usually clearer. In seeds, API debugging, and repository docs, the name **implementation** and `Client-Key` match the product.

### Webshop

The part of the public site where people browse **provider** offers and products, make reservations, and spend **vouchers** or balances. Optional; depends on fund configuration. Implemented in the `webshop` frontend package, but that package also serves non-shop pages (funds, requests, regelingencheck).

**Note:** Use **webshop flows** (or “offer and redemption”) when you mean this slice only. For fund info pages, regelingencheck, or payout-only journeys, **website** is usually clearer. Repository paths and local setup URLs use **webshop** (for example `webshop.general`).

### Dashboard

The sponsor, provider, and validator apps where **employees** manage funds, products, requests, and related work.

**Note:** **Admin panel** (for example “sponsor admin panel”) is usually clearer in docs and client-facing copy. URLs, routes, and folders use **dashboard** (for example `dashboard.sponsor`, `Client-Type: sponsor`).

## Support and flows

What gets issued or which process runs.

### Voucher

Support issued to someone through a **fund**. The main kinds are:

- **Budget voucher** — an amount the **support recipient** can spend at participating **providers** (the remaining spendable amount is often called the **balance** on that voucher).
- **Product voucher** — support tied to one specific offer or reservation (often created from a budget voucher when someone books in the **webshop**).
- **Payout** — money sent to a bank account; not used as a scannable pass at a shop (see **Payout** below).

The same fund can use more than one kind. **Reimbursements** are separate claims against a budget voucher (for example after paying themselves); the product does not use the term “declaration” for that.

### Voucher access code

Each redeemable voucher gets two secret codes when it is created. They identify the same voucher but are used in different situations:

- **Direct-use code** — in QR codes that **employees** send or export, when a **provider** scans a pass, and in many system e-mails.
- **Share code** — shown to the **support recipient** on the **website** when they share or assign the voucher; the recipient may need an extra confirmation step first.

**Note:** In technical material you may see **voucher token** or **token address** for the same idea. There are always two codes per voucher, not one.

### QR code (voucher)

A QR image that encodes a **voucher access code** so a **provider** (or the system) can look up the voucher. Whether QR codes are shown at all depends on **fund** settings.

The same QR can exist in several forms at once—for example on the **website** while logged in, in an e-mail, on a printout, in a sponsor export, or on optional card artwork. That is one pass in multiple places; a photo or forward copy is still the same access, not a second balance.

Scanning a **budget voucher** QR refers to that voucher’s spendable amount (within fund rules). Scanning a **product voucher** QR refers to that specific offer. A “discount” on an offer comes from how the **provider** priced the product, not from the QR itself.

### Physical card

An optional plastic or paper pass linked to the same **voucher**, with its own short number printed on the card. **Providers** can use either that number or a **QR code (voucher)** to find the voucher. A card does not create extra money or a second voucher—only another way to carry the same support, when the **fund** allows physical cards.

### Payout

Support paid out to a bank account (IBAN) instead of a scannable budget or product pass. Configured per **fund**, sometimes alongside redeemable **vouchers**.

### Fund request

An application or request flow through which an **applicant** asks for support from a fund; validators or sponsors may approve or reject it.

### Digest

A bundled email that summarizes recent activity instead of sending one message per event. See [features/digest-emails.md](features/digest-emails.md).
