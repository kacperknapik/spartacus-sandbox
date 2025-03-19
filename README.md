# Spartacus training sandbox

## Prerequisites

- **Node.js** ^20.9.0 (check `.nvmrc` file for more information)
- **Angular CLI** 17.3.11
- **NPM** 10.9.2
- **Docker** (optional, without Docker, you need to install `verdaccio` locally)

## Installation

### Verdaccio - local registry

- Run `verdaccio` in Docker - it will expose a local registry at `http://localhost:4873`
  - start at the root of this project
  - `cd verdaccio`
  - `docker compose up -d` (or `docker-compose up -d` for older versions)
- Set up local registry
  - check whether local registry is accessible [http://localhost:4873]()
  - create registry user - `npm adduser --registry http://localhost:4873` and follow interactive commands
  - (optional) if the user already exists, you can login - `npm login --registry http://localhost:4873`

### Spartacus source core

- Set up Spartacus source code
  - start at the root of this project
  - `git clone https://github.com/SAP/spartacus.git spartacus-source`
  - `cd spartacus-source`
  - `git checkout release-2211.32.1`
  - `npm install && npm run build:libs`
  - `npm run start`
  - visit `http://localhost:4200` to check if Spartacus core works correctly
  - after confirming that Spartacus core works, you can close the terminal

### Publishing to local registry

- Publish Spartacus source code to local registry
  - start inside of `spartacus-source/` folder
  - `npm i -g ts-node`
  - `ts-node ./tools/schematics/testing`
  - select `publish`

### Install Spartacus as dependency for Angular application

- Prepare Angular application
  - start at the root of this project
  - (optional) `npm i -g @angular/cli@17.3.11`
  - `ng new spartacus-app --style=scss --routing=false --standalone=false`
  - select `no` for SSR
  - `cd spartacus-app`
- Configure local registry
  - create `.npmrc` file (in `spartacus-app` directory)
  - add configuration - `@spartacus:registry=http://localhost:4873`
- Install Spartacus
  - `ng add @spartacus/schematics`
  - press `enter` when asked for libraries to install
  - `npm i`
  - change `baseUrl` in `spartacus-app/src/app/spartacus/spartacus-configuration.module.ts` to `https://composable-storefront-demo.eastus.cloudapp.azure.com:8443` (Spartacus demo instance)
  - `npm start`
 
## Known issues

- For Windows users:
  - You might need to replace `../../node_modules/.bin/` with `''` (empty string), because Windows is not recognizing relative paths to npm packages
  - There might be an issue with styles, because of forward/backward slash - check `/styles/spartacus/*.scss` files
  - Installation might fail if you have spaces in your folder names

## License

This project uses components licensed under the Apache License 2.0.

- [Spartacus](https://github.com/SAP/spartacus) - Used for setting up a sandbox environment for training purposes

For more details, see the [LICENSE](./LICENSE.md) and [NOTICE](./NOTICE.md) files.
