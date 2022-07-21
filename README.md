# Vulcan Eurodance Stack

![The Remix Eurodance Stack 🇪🇺 🐸 🛵](https://raw.githubusercontent.com/VulcanJS/vulcan-npm/main/docusaurus/static/img/remix/eurodance-banner.jpeg)
_Based on Remix [Indie Stack](https://github.com/remix-run/indie-stack)_

<!--![The Remix Eurodance Stack](https://repository-images.githubusercontent.com/465928257/a241fa49-bd4d-485a-a2a5-5cb8e4ee0abf)-->

Eurodance Stack is the Remix stack for **GraphQL** developers.

Learn more about [Remix Stacks](https://remix.run/stacks).

```
npx create-remix --template VulcanJS/eurodance-stack
```

You can also run Eurodance Stack on [CodeSandbox Projects](https://projects.codesandbox.io/). As it is currently in beta (06/2022), you need to ask for an early access, and then search for "VulcanJS/eurodance-stack" when creating your project.

**NOTE: if you clone this repo directly, and don't use `create-remix`, you will need to run `yarn remix init` manually after the first install!**

## What's in the stack

### From Vulcan (work in progress):

**GraphQL**

- Invisible GraphQL: work transparently with GraphQL, without ever depending client-side

**Vercel deployment**

Remix stacks hosts on Fly out-of-the-box.
For historical reasons Vulcan prefers Vercel, but you can deploy your Remix app almost anywhere very easily.

- We already setup remix to run a specific "vercel.server.js" on Vercel
- We already set a minimum vercel.json
- Create a new project on Vercel or run Vercel CLI to setup the project: `vercel` (answer the questions)
- IMPORTANT: add a `SESSION_SECRET` to your Vercel app secrets, to do this you can run the following commands:
  ```sh
  vercel env add SESSION_SECRET=$(openssl rand -hex 32)
  ```
  If you don't have openssl installed, you can also use [1password](https://1password.com/password-generator/) to generate a random secret, just replace `$(openssl rand -hex 32)` with the generated secret.
- If using SQLite, set `DATABASE_URL` to `file:./tmp/data/data.db?connection_limit=1`. **Important note: the data will not persist!**
**This is only for demonstration purpose.** You should find an host that supports SQLite or wait until we introduce MongoDB in this stack.

**CodeSandbox Projects**

Eurodance supports being installed as a CodeSandbox Project (beta feature).

**Storybook**

Basic setup with Vite, but also [@storybook/testing-react](https://storybook.js.org/addons/@storybook/testing-react). This brilliant addon lets you import your stories in unit tests.

***Embrace Story Driven Development:***:

1. Develop components visually in isolation with Storybook
2. Share with your designers and teammates to validate the behaviour and appearance
3. Import your stories in a Vitest unit test
4. Automate testing and prevent regression with React Testing Library

### Incoming

**MongoDB supports (via Prisma)**

Remix stacks works with Prisma out-of-the-box, and various databases.
For historical reasons, Vulcan prefers Mongo, but you can switch very easily to any database.

**Advanced GraphQL**

- A GraphQL resource route with [GraphQL Yoga](https://www.graphql-yoga.com/)
- Vulcan Fire Engine


[And many others, follow the umbrella ticket on GitHub to learn more about our roadmap.](https://github.com/VulcanJS/vulcan-npm/issues/117)

### Features inherited from Remix Indie stack:

- [Fly app deployment](https://fly.io) with [Docker](https://www.docker.com/)
- Production-ready [SQLite Database](https://sqlite.org)
- Healthcheck endpoint for [Fly backups region fallbacks](https://fly.io/docs/reference/configuration/#services-http_checks)
- [GitHub Actions](https://github.com/features/actions) for deploy on merge to production and staging environments
- Email/Password Authentication with [cookie-based sessions](https://remix.run/docs/en/v1/api/remix#createcookiesessionstorage)
- Database ORM with [Prisma](https://prisma.io)
- Styling with [Tailwind](https://tailwindcss.com/)
- End-to-end testing with [Cypress](https://cypress.io)
- Local third party request mocking with [MSW](https://mswjs.io)
- Unit testing with [Vitest](https://vitest.dev) and [Testing Library](https://testing-library.com)
- Code formatting with [Prettier](https://prettier.io)
- Linting with [ESLint](https://eslint.org)
- Static Types with [TypeScript](https://typescriptlang.org)

Not a fan of bits of the stack? Fork it, change it, and use `npx create-remix --template your/repo`! Make it your own.

## Quickstart

Click this button to create a [Gitpod](https://gitpod.io) workspace with the project set up and Fly pre-installed

[![Gitpod Ready-to-Code](https://img.shields.io/badge/Gitpod-Ready--to--Code-blue?logo=gitpod)](https://gitpod.io/from-referrer/)

## Development

- This step only applies if you've opted out of having the CLI install dependencies for you:
  
  ```sh
  npx remix init
  ```

- Initial setup: _If you just generated this project, this step has been done for you._

  ```sh
  npm run setup
  ```

- Start dev server:

  ```sh
  npm run dev
  ```

This starts your app in development mode, rebuilding assets on file changes.

The database seed script creates a new user with some data you can use to get started:

- Email: `rachel@remix.run`
- Password: `racheliscool`

### Relevant code:

This is a pretty simple note-taking app, but it's a good example of how you can build a full stack app with Prisma and Remix. The main functionality is creating users, logging in and out, and creating and deleting notes.

- creating users, and logging in and out [./app/models/user.server.ts](./app/models/user.server.ts)
- user sessions, and verifying them [./app/session.server.ts](./app/session.server.ts)
- creating, and deleting notes [./app/models/note.server.ts](./app/models/note.server.ts)

## Deployment with Fly

**NOTE: in Eurodance stack we favour Vercel deployment. We have commented out Fly related setup.**
**You can easily remove Vercel and reenable FLy.**

This Remix Stack comes with two GitHub Actions that handle automatically deploying your app to production and staging environments.

Prior to your first deployment, you'll need to do a few things:

- [Install Fly](https://fly.io/docs/getting-started/installing-flyctl/)

- Sign up and log in to Fly

  ```sh
  fly auth signup
  ```

  > **Note:** If you have more than one Fly account, ensure that you are signed into the same account in the Fly CLI as you are in the browser. In your terminal, run `fly auth whoami` and ensure the email matches the Fly account signed into the browser.

- Create two apps on Fly, one for staging and one for production:

  ```sh
  fly create vulcan-remix
  fly create vulcan-remix-staging
  ```

  - Initialize Git.

  ```sh
  git init
  ```

- Create a new [GitHub Repository](https://repo.new), and then add it as the remote for your project. **Do not push your app yet!**

  ```sh
  git remote add origin <ORIGIN_URL>
  ```

- Add a `FLY_API_TOKEN` to your GitHub repo. To do this, go to your user settings on Fly and create a new [token](https://web.fly.io/user/personal_access_tokens/new), then add it to [your repo secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) with the name `FLY_API_TOKEN`.

- Add a `SESSION_SECRET` to your fly app secrets, to do this you can run the following commands:

  ```sh
  fly secrets set SESSION_SECRET=$(openssl rand -hex 32) --app vulcan-remix
  fly secrets set SESSION_SECRET=$(openssl rand -hex 32) --app vulcan-remix-staging
  ```

  If you don't have openssl installed, you can also use [1password](https://1password.com/password-generator/) to generate a random secret, just replace `$(openssl rand -hex 32)` with the generated secret.

- Create a persistent volume for the sqlite database for both your staging and production environments. Run the following:

  ```sh
  fly volumes create data --size 1 --app vulcan-remix
  fly volumes create data --size 1 --app vulcan-remix-staging
  ```

Now that everything is set up you can commit and push your changes to your repo. Every commit to your `main` branch will trigger a deployment to your production environment, and every commit to your `dev` branch will trigger a deployment to your staging environment.

### Connecting to your database

The sqlite database lives at `/data/sqlite.db` in your deployed application. You can connect to the live database by running `fly ssh console -C database-cli`.

### Getting Help with Deployment

If you run into any issues deploying to Fly, make sure you've followed all of the steps above and if you have, then post as many details about your deployment (including your app name) to [the Fly support community](https://community.fly.io). They're normally pretty responsive over there and hopefully can help resolve any of your deployment issues and questions.

## GitHub Actions

We use GitHub Actions for continuous integration and deployment. Anything that gets into the `main` branch will be deployed to production after running tests/build/etc. Anything in the `dev` branch will be deployed to staging.

## Testing

### Cypress

We use Cypress for our End-to-End tests in this project. You'll find those in the `cypress` directory. As you make changes, add to an existing file or create a new file in the `cypress/e2e` directory to test your changes.

We use [`@testing-library/cypress`](https://testing-library.com/cypress) for selecting elements on the page semantically.

To run these tests in development, run `npm run test:e2e:dev` which will start the dev server for the app as well as the Cypress client. Make sure the database is running in docker as described above.

We have a utility for testing authenticated features without having to go through the login flow:

```ts
cy.login();
// you are now logged in as a new user
```

We also have a utility to auto-delete the user at the end of your test. Just make sure to add this in each test file:

```ts
afterEach(() => {
  cy.cleanupUser();
});
```

That way, we can keep your local db clean and keep your tests isolated from one another.

### Vitest

For lower level tests of utilities and individual components, we use `vitest`. We have DOM-specific assertion helpers via [`@testing-library/jest-dom`](https://testing-library.com/jest-dom).

### Type Checking

This project uses TypeScript. It's recommended to get TypeScript set up for your editor to get a really great in-editor experience with type checking and auto-complete. To run type checking across the whole project, run `npm run typecheck`.

### Linting

This project uses ESLint for linting. That is configured in `.eslintrc.js`.

### Formatting

We use [Prettier](https://prettier.io/) for auto-formatting in this project. It's recommended to install an editor plugin (like the [VSCode Prettier plugin](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)) to get auto-formatting on save. There's also a `npm run format` script you can run to format all files in the project.

---

## They support Vulcan

### Contributors

This project exists thanks to all the people who contribute.

<a href="https://github.com/VulcanJS/vulcan-next/graphs/contributors"><img src="https://opencollective.com/vulcan/contributors.svg?width=890&button=false" /></a>

### Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/vulcan#contribute)]

<a href="https://opencollective.com/vulcan#contributors" target="_blank"><img src="https://opencollective.com/vulcan/backers.svg?width=890"/></a>

### Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/vulcan#contribute)]

<a href="https://opencollective.com/vulcan#contributors" target="_blank"><img src="https://opencollective.com/vulcan/sponsors.svg?width=890"/></a>

### Technical supports

They give time and share knowledge to support the project.

<a href="https://aplines.com" target="_blank" rel="noopener noreferrer">
<img src="https://aplines.com/wp-content/uploads/2022/03/cropped-aplines-logo.png" alt="aplines" height="75"/>
</a>
<a href="https://www.lbke.fr" target="_blank" rel="noopener noreferrer">
<img src="https://www.lbke.fr/img/logo-md.png" height="75" alt="lbke" />
</a>
<a href="https://letter.so/" target="_blank" rel="noopener noreferrer">
<img src="https://github.com/VulcanJS/vulcan-next/blob/devel/public/img/letter-96x96.png?raw=true" height="75" alt="lette.so" />
</a>

## Other cool stuff that inspires us

- [remix-graphql](https://github.com/thomasheyenbrock/remix-graphql) served as a basis to setup GraphQL in Remix

---

[![Powered by Vercel](https://www.datocms-assets.com/31049/1618983297-powered-by-vercel.svg)](https://vercel.com?utm_source=vulcan&utm_campaign=oss)
