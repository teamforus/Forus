# Contributing to Forus

Thank you for your interest in contributing to Forus.

This repository is the **main entry point** for the Forus platform. It acts as an umbrella repository that references the backend and frontend codebases, which live in separate repositories.

This document explains how the repositories relate to each other, how to set up a local development environment, and where to propose changes.

---

## Repository structure

The Forus platform consists of multiple repositories:

- **teamforus/Forus**  
  The main repository.  
  Acts as an entry point and pins specific versions of the backend and frontend repositories. The main `Forus` repository contains project-level documentation (such as this contributing guide) and references to the other repositories. It does not contain application code directly. You will also find the releases of Forus here: https://github.com/teamforus/Forus/releases

- **teamforus/forus-backend**  
  Contains the backend application, backend-specific documentation, and Docker configuration.

- **teamforus/forus-frontend-react**  
  Contains the frontend application, frontend-specific documentation, and its Docker configuration.

---

## Where to propose changes

### Issues

All issues must be opened in the main `teamforus/Forus` repository. The issue functionality is disabled in the other repositories.

#### How to create a good issue

When opening an issue, please provide the following information:

1. **Clear title**: Use a descriptive title that summarizes the problem or request
2. **Specify the area**: Indicate which part of the platform the issue relates to:
   - Backend (`teamforus/forus-backend`)
   - Frontend (`teamforus/forus-frontend-react`)
   - General/main repository (`teamforus/Forus`)
   - Documentation (see [Documentation](#documentation) section below for more details)
3. **Description**: Provide a clear and detailed description of:
   - What the issue is (bug, feature request, question, etc.)
   - What you expected to happen
   - What actually happened (for bugs)
   - Steps to reproduce (for bugs)
   - Environment details (OS, browser, versions, etc.) when relevant
4. **Screenshots or examples**: Include screenshots, error messages, or code examples when helpful
5. **Related issues**: Reference any related issues or pull requests if applicable

#### Best practices

- **Search first**: Check if a similar issue already exists before creating a new one
- **Be specific**: Provide enough detail for others to understand and potentially help with the issue
- **Use labels appropriately**: If you have permission, add relevant labels to help categorize the issue
- **Respond to questions**: Be ready to provide additional information if maintainers ask for clarification
- **Update status**: If you find a solution or the issue is resolved, update the issue accordingly

### Creating branches

Before creating a branch, first determine which repository your changes affect:
- **Main repository** (`teamforus/Forus`): For project-level documentation (like this file)
- **Backend repository** (`teamforus/forus-backend`): For backend code and backend-specific documentation
- **Frontend repository** (`teamforus/forus-frontend-react`): For frontend code and frontend-specific documentation

#### Contributing to the main repository

For changes to the main repository (e.g., this contributing guide):

1. Navigate to the `teamforus/Forus` repository
2. Make sure you're on the `master` branch and it's up to date
3. Create a new branch following the [branch naming conventions](#branch-naming-conventions)
4. Make your changes using your preferred method (GitHub web interface, IDE, command line, etc.)
5. Commit and push your changes to your new branch
6. Create a pull request from your branch to `master`
7. Maintainers will review your pull request—make sure to respond to any feedback
8. Once accepted, your changes will be merged into `master`

**Note**: You cannot make direct changes to the backend or frontend repositories from the main repository. You need to work directly in those repositories.

#### Contributing to backend or frontend repositories

For changes to backend or frontend code, follow the general information below and check the detailed contributing guides in each repository:
- Backend: See `CONTRIBUTING.md` in the `teamforus/forus-backend` repository
- Frontend: See `CONTRIBUTING.md` in the `teamforus/forus-frontend-react` repository

Create branches for each change from the appropriate repository (backend or frontend).

#### Base branch

The base branch depends on which repository you're working in:

- **Main repository** (`teamforus/Forus`): Create branches from `master`
- **Backend and frontend repositories**: Create branches from `develop`

The `develop` branch is the main development branch in the backend and frontend repositories where all new features and fixes are integrated before being merged to `master`.

**For backend/frontend repositories:**

```sh
# Make sure you're on the develop branch and it's up to date
git checkout develop
git pull origin develop

# Create a new branch for your changes
git checkout -b your-branch-name
```

**For the main repository:**

```sh
# Make sure you're on the master branch and it's up to date
git checkout master
git pull origin master

# Create a new branch for your changes
git checkout -b your-branch-name
```

#### Branch naming conventions

While there are no strict requirements, following these conventions helps keep the repository organized:

- Use descriptive, lowercase names with hyphens (kebab-case), do not use spaces.
- Include the issue number if your branch addresses a specific issue
- Keep names concise but meaningful

**Examples of good branch names:**
- `feature/add-user-authentication`
- `fix/payment-processing-bug`
- `bugfix/123-fix-login-error` (if issue #123 exists)
- `refactor/improve-api-response-structure`
- `docs/update-contributing-guide`

**Avoid:**
- Generic names like `fix` or `update`
- Names with spaces or special characters
- Very long names that are hard to read

#### Best practices

- **One branch per feature/fix**: Keep branches focused on a single concern
- **Keep branches up to date**: Regularly merge or rebase `develop` into your branch to avoid conflicts
- **Delete merged branches**: After your pull request is merged, delete the branch locally and remotely

### Pull Requests

Pull requests should be opened in the appropriate repository:
- **Backend changes**: Open PRs in `teamforus/forus-backend`
- **Frontend changes**: Open PRs in `teamforus/forus-frontend-react`

### Documentation

Documentation improvements are welcome and follow the same contribution process as code changes.

#### Where documentation lives

Documentation is located in multiple places:
- **Main repository** (`teamforus/Forus`): This `CONTRIBUTING.md` file and other project-level documentation
- **Backend repository** (`teamforus/forus-backend`): `readme.md`, `readme-docker.md`, and other backend-specific documentation
- **Frontend repository** (`teamforus/forus-frontend-react`): `readme.md`, `readme-docker.md`, and other frontend-specific documentation

#### Requesting documentation changes

If you notice documentation that is unclear, outdated, or missing, you can:

1. **Open an issue** in the main `teamforus/Forus` repository describing:
   - What documentation needs improvement
   - Which repository it's in (or if it's in the main repo)
   - What specific changes you'd like to see
   - Why the change would be helpful

2. **Tag the issue appropriately** by specifying whether it relates to:
   - Backend documentation
   - Frontend documentation
   - General/main repository documentation

#### Making documentation changes

Documentation changes follow the same process as code changes:

1. **Create a branch** from `develop` in the appropriate repository (use the `docs/` prefix in your branch name, e.g., `docs/improve-docker-setup-instructions`)

2. **Make your changes** to the relevant documentation file(s)

3. **Open a pull request** in the repository where the documentation lives:
   - Main repository documentation → PR in `teamforus/Forus`
   - Backend documentation → PR in `teamforus/forus-backend`
   - Frontend documentation → PR in `teamforus/forus-frontend-react`

4. **Reference related issues** in your PR description if applicable

Documentation improvements don't require code changes, but they are just as valuable to the project!

---

## Local development

### Supported local development environments

The Forus platform can be run locally using different approaches. The sections below describe what is supported and what is documented.

- **Docker Desktop (Windows, macOS, Linux Desktop)**  
  Recommended for most developers.  
  This documentation primarily targets this setup and includes Docker Desktop-specific guidance and optimizations.

- **Native Docker (without Docker Desktop)**  
  Supported.  
  Performance characteristics differ from Docker Desktop and some optimizations described here may not be required.  
  Setup steps are largely the same as for Docker Desktop.

- **Fully local setup without Docker**  
  Possible, but not officially documented or supported at this time. 

### Docker Desktop (recommended method)

This is the recommended way to run Forus locally for most developers.

The Forus platform consists of a backend and a frontend, each maintained in its own repository with its own Docker configuration and environment files. Both must be set up separately.

#### High-level steps

1. **Install Docker Desktop**
   Download and install Docker Desktop from:
   [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
   Make sure Docker Desktop is running before continuing.

2. **Clone the repositories**

Clone the backend and frontend repositories using your preferred method.
Using HTTPS is often the easiest option, as it will prompt you to log in via the browser.

Example using HTTPS:

   ```sh
   git clone https://github.com/teamforus/forus-backend.git
   git clone https://github.com/teamforus/forus-frontend-react.git
   ```

Consider using SSH instead if:
- You contribute to these repositories frequently
- You want to avoid browser-based authentication prompts
- You already have an SSH key configured in your GitHub account


3. **Set up the backend first**

   ```sh
   cd forus-backend
   ```

- **Create the backend environment file**  
  Copy `.env.example` to `.env` in the same directory and update values where needed
  (for example database credentials, domains, external services).

- **Start the backend containers**  
  Start the containers either:
  - From the terminal:
    ```sh
    docker compose up -d
    ```
  - Or via the Docker Desktop interface.

- **Install backend dependencies**  
  Install PHP dependencies inside the running container:
  ```sh
  docker compose exec app composer install
  ```

- **Run database migrations and seeders**  
  Create the database structure and populate required system tables:
  ```sh
  docker compose exec app php artisan migrate
  docker compose exec app php artisan db:seed
  ```

- **Generate the application key**  
  Generate the Laravel application key required for encryption and sessions:
  ```sh
  docker compose exec app php artisan key:generate
  ```

Detailed backend setup steps are documented in the `readme-docker.md` file in the backend repository.

At this point the backend API should be running and reachable. Run the following command from any terminal:

```sh
curl http://localhost:8000
```

Expected result: You receive an HTTP response.

This confirms the backend container is running and accepting requests.


4. **Set up the frontend**

   ```sh
   cd ../forus-frontend-react
   ```

- **Create the frontend environment file**  
  Copy `env.example.js` to `env.js` in the same directory:
  ```sh
  cp env.example.js env.js
  ```

- **Configure the frontend environment**  
  Open `env.js` and configure the following:
  - Set the API base URL to the backend, for example:
    ```js
    const api_url = 'http://localhost:8000/api/v1';
    ```
  - Configure which fronts to enable: The `enableOnly` array controls which webshops and dashboards are built. By default, it includes `webshop.general`, `dashboard.sponsor`, `dashboard.provider`, and `dashboard.validator`. You can add more webshop implementations (like `webshop.nijmegen`, `webshop.potjeswijzer`, etc.) to the `fronts` object and include them in `enableOnly` if they exist in your backend.

- **Start the frontend containers**  
  Start the containers either:
  - From the terminal:
    ```sh
    docker compose up -d
    ```
  - Or via the Docker Desktop interface.

- **Install frontend dependencies**  
  Install Node.js dependencies inside the running container:
  ```sh
  docker compose exec app sh -c "npm install"
  ```

- **Build or start the frontend**  
  Choose one of the following:
  - Start the development server (auto rebuilds on changes):
    ```sh
    docker compose exec app sh -c "npm run start"
    ```
  - Build the frontend once (static build):
    ```sh
    docker compose exec app sh -c "npm run build"
    ```

Detailed frontend setup steps are documented in the `readme-docker.md` file in the frontend repository.


5. **Verify**

   * Backend API is reachable (for example on `http://localhost:8000`)
   * Frontend loads correctly in the browser. Navigate to the following URLs to verify:
     - `http://localhost:3000/webshop.general` - General webshop (when using development server)
     - `http://localhost:3000/dashboard.sponsor` - Sponsor dashboard
     - `http://localhost:3000/dashboard.provider` - Provider dashboard
     - `http://localhost:3000/dashboard.validator` - Validator dashboard
     
     If you've configured additional webshops in `env.js`, you can access them at `http://localhost:3000/webshop.<name>` (replace `<name>` with the webshop key you configured).

#### Notes

* Backend and frontend each have their own `docker-compose.yml`.
  Always run Docker commands from inside the correct repository directory.
* The backend should be started first. The frontend depends on the backend API.
* Dependency installation (`composer install`, `npm install`) is executed inside containers.

---

## Contribution guidelines

- Create an issue before submitting a pull request when possible.
- Clearly describe the problem, environment, and reproduction steps.
- Keep pull requests focused and scoped to a single concern.

---

## Getting help

If you have questions, need clarification, or want to discuss contributions, feel free to reach out on our Discord server:

- [Discord Community](https://discord.forus.io/)

---

## License

By contributing, you agree that your contributions will be licensed under the GNU Affero General Public License v3 (AGPLv3), consistent with the rest of the project.

For the full license text, see [LICENSE](https://github.com/teamforus/Forus/blob/master/LICENSE).
