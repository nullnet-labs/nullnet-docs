# Starting the Next.JS Frontend

### Prerequisites

- [Node.JS](https://nodejs.org/) version 20.9+
  - can check current version using the `node --version` command

## My Setup Steps

### create-next-app

- In a terminal, cd to the desired project folder, and use the command: `npx create-next-app@latest <your project name>`
- (If create-next-app isn't already on your system)
  ```
  Need to install the following packages:
  create-next-app@<version number>
  Ok to proceed? (y)
  ```
    - Just press Enter
    - My setup gave the version number `16.2.6`
- `Would you like to use the recommended Next.js defaults?`
  - No, customize settings
- `Would you like to use TypeScript?`
  - Yes
- `Which linter would you like to use?`
  - ESLint
- `Would you like to use React Compiler?`
  - No
- `Would you like to use Tailwind CSS?`
  - Yes
- ``Would you like your code inside a `src/` directory?``
  - Yes
- `Would you like to use App Router? (recommended)`
  - Yes
- ``Would you like to customize the import alias (`@/*` by default)?``
  - No
- `Would you like to include AGENTS.md to guide coding agents to write up-to-date Next.js code?`
  - No
  - There's no harm in picking "Yes" here, even if AI coding isn't being used. However, for my case, this just adds a file that I'm not using.

### Configuration

- Optionally, add an ESLint auto-correct script to the `package.json` file:
  ```
  {
    ...
    "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start",
      "lint": "eslint",
      "lint:fix": "eslint --fix"
    }
    ...
  }
  ```
  - This both runs ESLint and fixes any code smells it finds
  - Can be useful for small quick fixes before code merges, but don't lean on it so often that you don't know what it's linting for. By using "lint" without "fix," you can see what ESLint is calling out without fixing it automatically, allowing it to be reviewed & fixed manually. This will allow for cleaner coding and less retroactive fixing in the future.

- For VSCode: Select the search bar at the top -> `Show Run Commands (Ctrl + Shift + P)` -> `Typescript: Select TypeScript Version...` -> `Use Workspace Version`

- In `tsconfig.json`, you can optionally configure imports to be less verbose (for instance, changing '../../../components/button' to '@/components/button'). For my `tsconfig.json` setup, I changed this:
  ```
    "paths": {
      "@/styles/": ["./src/*"]
    }
  ```
  to this:
  ```
    "baseUrl": "src/",
    "paths": {
      "@/styles/": ["styles/*"],
      "@/components/*": ["components/*"]
    }
  ```
  - Each `paths` value is relative to the `baseUrl` location.

## Run Check

Ensure the application runs.

- Run the command `npm run dev`
- Use a Web browser to visit [http://localhost:3000/](http://localhost:3000/) to see if the application is reachable

---

<details>
  <summary>
    Click here to see my initial package.json
  </summary>

```
{
    "name": "nullnet-frontend",
    "version": "0.1.0",
    "private": true,
    "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start",
      "lint": "eslint",
      "lint:fix": "eslint --fix"
    },
    "dependencies": {
      "next": "^16.2.6",
      "react": "^19.2.6",
      "react-dom": "^19.2.6"
    },
    "devDependencies": {
      "@tailwindcss/postcss": "^4",
      "@types/node": "^20",
      "@types/react": "^19",
      "@types/react-dom": "^19",
      "eslint": "^9",
      "eslint-config-next": "16.2.6",
      "tailwindcss": "^4",
      "typescript": "^5"
    }
}
```
</details>

<details>
  <summary>
    Click here to see my initial .gitignore
  </summary>

```
# dependencies
/node_modules
/.pnp
.pnp.*
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/versions

# testing
/coverage

# next.js
/.next/
/out/

# production
/build

# misc
.DS_Store
*.pem

# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.pnpm-debug.log*

# local env files
# .env*.local
# .env
.env*
!.env.template

# vercel
.vercel

# typescript
*.tsbuildinfo
next-env.d.ts

```
</details>
