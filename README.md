<p align="center">
  <a href="https://www.forus.io">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="docs/assets/logo-white.svg">
      <source media="(prefers-color-scheme: light)" srcset="docs/assets/logo-black.svg">
      <img src="docs/assets/logo-black.svg" width="280" alt="Forus">
    </picture>
  </a>
</p>

<p align="center">
  <a href="./LICENSE"><img src="https://img.shields.io/github/license/teamforus/Forus" alt="License"></a>
  <a href="https://github.com/teamforus/Forus/releases"><img src="https://img.shields.io/github/v/release/teamforus/Forus" alt="Release"></a>
  <a href="https://github.com/teamforus/forus-backend/blob/develop/composer.json"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Fteamforus%2Fforus-backend%2Fdevelop%2Fcomposer.json&query=%24.require.php&label=PHP" alt="PHP"></a>
  <a href="https://github.com/teamforus/forus-backend/blob/develop/composer.json"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Fteamforus%2Fforus-backend%2Fdevelop%2Fcomposer.json&query=%24.require%5B%22laravel%2Fframework%22%5D&label=Laravel" alt="Laravel"></a>
  <a href="https://github.com/teamforus/forus-frontend/blob/develop/package.json"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Fteamforus%2Fforus-frontend%2Fdevelop%2Fpackage.json&query=%24.dependencies.react&label=React" alt="React"></a>
</p>

## About
Forus is an open-source multi-tenant SaaS platform that provides organizations such as governments and charities with a wide range of functionalities to issue and manage social benefit policies.

The platform can be used for policies that address a broad range of social challenges, including low income, social inclusion, access to transport, health and wellbeing, participation in sports and culture, support for local entrepreneurship, and sustainability.

Effective social benefit policies require a full operational process. Organizations need to reach and inform target groups, explain eligibility criteria, provide secure and accessible application flows, validate applications and data, issue support in the required form, monitor how funds are used, prevent fraud and abuse, and report on financial activity and policy impact.

Support can be provided through Forus in different ways. This can include a direct monetary payout, a reimbursement after the applicant submits proof of costs, a QR voucher with a balance that can be spent with approved providers, or a discount.

## How it works

Forus runs as one shared platform, a multi-tenant SaaS application. A single backend stores organizations, funds, identities, applications, issued support, transactions, and related configuration.

Users can create an individual account, an organizational account, or both, depending on their role. Organizational users can access role-specific admin panels. Through these admin panels, an organization can create and manage funds and configure the rules and settings for specific policies. A public website reaches a target group. It is available once an implementation is in place. Setting up that implementation is a configuration step, not an action in the admin panel. The sponsor maintains the website from the admin panel.

A typical flow starts with a sponsor organization configuring a fund in the sponsor admin panel. The fund defines the policy rules, eligibility criteria, budget, providers, and method of issuing support. The fund is then published on a public website, where an applicant can sign in, view available support, check eligibility, and submit a fund request. When validation is enabled, validator employees review the request and approve or reject it.

After approval, support is issued in the configured form, such as a voucher, payout, reimbursement or discount. The applicant can use the support on the website through webshop flows, or with a provider that redeems the voucher at the counter. Not every fund uses every step. The configuration determines which parts of the process apply.

## Forus main repo
This repository is the contributor entry point for the Forus open-source platform. It contains documentation, local setup guides, and links to the backend and frontend source as submodules.

## Repository structure

- Main repository: `Forus` main repository
- Submodule: `forus-backend`
- Submodule: `forus-frontend`
## Start here

1. Make sure you have a general understanding of the structure of the project documentation [docs/index.md](docs/index.md)
2. Run Forus on your machine: install the required tools, clone the repository (including submodules), and start the backend and frontend — [docs/local-setup.md](docs/local-setup.md)
3. Seed the database with sample organizations, funds, and identities so the website and admin panels show real data and you can sign in locally. [docs/seeding-test-data.md](docs/seeding-test-data.md)
4. Sign into the system as a user and check out the UI´s [docs/login-and-test-users.md](docs/login-and-test-users.md)
5. Learn how to make contributions to the codebase or documentation [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution workflow

## Project docs

| Document | Purpose |
| --- | --- |
| [docs/index.md](docs/index.md) | Documentation overview and reading order |
| [docs/local-setup.md](docs/local-setup.md) | Run the backend and frontend locally |
| [docs/seeding-test-data.md](docs/seeding-test-data.md) | Seed organizations, funds, and test identities |
| [docs/login-and-test-users.md](docs/login-and-test-users.md) | Local login flow and development token shortcut |
| [docs/glossary.md](docs/glossary.md) | Domain terms and recommended wording |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Basic contribution workflow |
| [backend/readme-docker.md](backend/readme-docker.md) | Backend Docker reference |
| [forus-frontend/readme-docker.md](forus-frontend/readme-docker.md) | Frontend Docker reference |
| [SECURITY.md](SECURITY.md) | Security issue reporting |

## Core domain model (quick overview)

### Identities represent natural persons in the system

An identity represents a natural person in the system. The same identity can act in different contexts, for example as a citizen or applicant, as an employee, or in relation to one or more organizations. Roles and permissions are structured around this identity.

### Organizations represent formal parties in the system

Organizations represent formal parties in the system, such as municipalities, sponsors, and providers. Depending on the setup, an organization can fulfill one or more roles, such as sponsor, provider, validator, or another operational role.

### Sponsor organizations create and manage funds

Organizations with a sponsor role create and manage funds. They typically define eligibility rules, choose allocation types, and manage communication and reporting.

### Provider organizations offer goods and services

Organizations with a provider role offer goods and services that can be made available through the platform. These can be used through configured flows, such as voucher redemption or other supported allocation types.

### Funds define the available support

A fund defines the support that can be issued. This includes who can apply, which type of support is available, where it can be used, and the period in which it is valid.

### Websites and implementations

A website is the public site for the people a sponsor wants to reach. Every public website uses the frontend in `forus-frontend`. A new website is not a new frontend repository.

The frontend loads one implementation at a time, using that implementation's key (`Client-Key`). An implementation holds the title, look, funds, sign-in, and pages for that website.

Particular existing websites also keep their own text in the frontend. That text is shown only when the implementation key matches. The pages and settings of every website still come from the implementation.

### Vouchers and other allocations are issued through the system

After approval, or through another configured flow such as direct allocation or prevalidation, support can be issued in different forms, such as vouchers, balances, payouts, or declarations.

### This is how the basic flow works

1. A sponsor organization defines a fund.
2. The fund is published on the website of an implementation.
3. An identity signs in and applies, or is granted access.
4. Support is issued.
5. Issued support is redeemed with providers or paid out through the configured flow.

## License

### 1. About Stichting Forus (the Forus Foundation)

Stichting Forus (hereafter: The Forus Foundation) is the developer of the Forus platform and the mobile application 'Me' (collectively hereinafter referred to as 'Software'). One of the goals of the Forus Foundation is providing its Software as open source. This is to ensure continuity for users, to prevent vendor lock-in and to develop better software through full transparency.

### 2. License Terms

Therefore, Forus Foundation permits use of the Software under the GNU Affero General Public License, Version 3, November 19, 2007 (hereinafter “AGPLv3”). The Forus Foundation retains its copyrights.

In other words, the Software is "free software" that you may distribute and/or modify in accordance with the AGPLv3 or (at your option) an official later version of the same terms, as published by the Free Software Foundation.

The Software is distributed in the hope that it will be useful, but WITHOUT WARRANTY OF ANY KIND; therefore without any implied warranty of sale or suitability for a particular purpose. See the AGPLv3 for further details.

You should have received a copy of the AGPLv3 with the Software. The AGPLv3 can also be found at https://www.gnu.org/licenses/agpl-3.0.nl.html.

### 3. Additional License Terms

In accordance with clause “7. Additional Terms” of the AGPLv3, the following additional terms apply to use of the Software and are inextricably linked to the license granted:

a. It is not permitted to remove source indications, such as indications that Stichting Forus are the creators of the original Software, from the Software and the source codes;

b. It is mandatory to provide notice in the case of modified versions of the Software that they contain changes from the original Software;

c. It is not permitted – other than under the obligations of the AGPLv3 and the Additional Licensing Terms – to use the trade names and brands of the Forus Foundation in the sale and marketing of derivative versions of the Software, unless the Forus Foundation has given written permission.;

d. Those who convey the Software, whether or not in modified form, to third parties indemnifies Forus Foundation and all natural persons who contributed to the creation of the Software against all claims by third parties, including claims relating to errors in the Software or data loss.
