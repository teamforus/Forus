# Forus

Forus is an open-source platform for governments and charitable organizations that want to issue and manage social benefit policies. This repository contains the main Laravel backend and React web frontend used to run and develop the platform.

The React frontend communicates with the Laravel backend API. For local development, both usually need to run at the same time.

## Repository structure

- `backend/`, Laravel (PHP) API
- `forus-frontend-react/`, React web applications for the webshop and the sponsor, provider, and validator dashboards

## Start here

- Review the documentation overview in [docs/index.md](docs/index.md)
- Set up the project locally with [docs/local-setup.md](docs/local-setup.md)
- Seed local test data with [docs/seeding-test-data.md](docs/seeding-test-data.md)
- Sign in locally using [docs/login-and-test-users.md](docs/login-and-test-users.md)
- See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution workflow

## Project docs

| Document | Purpose |
| --- | --- |
| [docs/index.md](docs/index.md) | Documentation overview and reading order |
| [docs/local-setup.md](docs/local-setup.md) | Run the backend and frontend locally |
| [docs/seeding-test-data.md](docs/seeding-test-data.md) | Seed organizations, funds, and test identities |
| [docs/login-and-test-users.md](docs/login-and-test-users.md) | Local login flow and development token shortcut |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Basic contribution workflow |
| [backend/readme-docker.md](backend/readme-docker.md) | Backend Docker reference |
| [forus-frontend-react/readme-docker.md](forus-frontend-react/readme-docker.md) | Frontend Docker reference |
| [backend/SECURITY.md](backend/SECURITY.md) | Security issue reporting |

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

### Websites and implementations provide access to the system

Users interact with the system through websites and other frontends connected to an implementation. This is where users view offers, sign in, apply for funds, and track issued support.

### Vouchers and other allocations are issued through the system

After approval, or through another configured flow such as direct allocation or prevalidation, support can be issued in different forms, such as vouchers, balances, payouts, or declarations.

### This is how the basic flow works

1. A sponsor organization defines a fund.
2. The fund is published through an implementation and its connected frontends.
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
