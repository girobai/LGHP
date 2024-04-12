# README

## setup

ref: <https://docs.astro.build/en/install/manual/>

### `package.json`

```json
{
  "name": "lghp",
  "type": "module",
  "version": "0.0.1",
  "scripts": {
    "dev": "astro dev",
    "start": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  }
}
```

<https://www.npmjs.com/package/astro>:

```sh
npm install astro
```

### `public/robots.txt`

```sh
# Example: Allow all bots to scan and index your site.
# Full syntax: https://developers.google.com/search/docs/advanced/robots/create-robots-txt
User-agent: *
Allow: /
```

### `astro.config.mjs`

```mjs
import { defineConfig } from "astro/config";

// https://astro.build/config
export default defineConfig({});
```

If you want to include [UI framework components](https://docs.astro.build/en/guides/framework-components/) such as React, Svelte, etc. or use other tools such as Tailwind or Partytown in your project, here is where you will [manually import and configure integrations](https://docs.astro.build/en/guides/integrations-guide/).

Read Astro’s [API configuration reference](https://docs.astro.build/en/reference/configuration-reference/) for more information.

### `tsconfig.json`

```json
{
  "extends": "astro/tsconfigs/strictest",
  "compilerOptions": {
    "baseUrl": "src"
  }
}
```

### `.gitignore`

```sh
# build output
dist/
# generated types
.astro/

.output/

# dependencies
node_modules/

# logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*


# environment variables
.env
.env.production

# macOS-specific files
.DS_Store

# ignore .astro directory
.astro

# ignore Jampack cache files
.jampack/

# yarn
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/sdks
!.yarn/versions
.pnp.*

# jetbrains setting folder
.idea/
```

### `src/env.d.ts`

Used to let TypeScript know about ambient types available in an Astro project:

```ts
/// <reference types="astro/client" />
```

Read Astro’s [TypeScript setup guide](https://docs.astro.build/en/guides/typescript/#setup) for more information.

### `.vscode/extensions.json`

```json
{
  "recommendations": ["astro-build.astro-vscode"]
}
```

### `.vscode/settings.json`

```json
{
  "workbench.colorTheme": "Abyss",
  "workbench.colorCustomizations": {
    "window.activeBorder": "#096df058",
    "titleBar.activeBackground": "#116de67e",
    "titleBar.inactiveBackground": "#096df034"
  }
}
```

### `src/pages/index.astro`

```astro
---
// Welcome to Astro! Everything between these triple-dash code fences
// is your "component frontmatter". It never runs in the browser.
console.log("This runs in your terminal, not the browser!");
---

<!-- Below is your "component template." It's just HTML, but with
     some magic sprinkled in to help you build great templates. -->
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro!</title>
  </head>
  <body>
    <h1>Astro oOo</h1>
  </body>
</html>
<style>
  h1 {
    color: orange;
  }
</style>

```

### Try locally

```sh
npm run dev
```

Check if any `src/pages/index.astro` update the dev preview
at: <http://localhost:4321/>.

### build it

```sh
npm run build
```

Check `dist/` output.

### Configure Astro for GitHub Pages Deploying to a github.io URL

`astro.config.mjs`:

```mjs
import { defineConfig } from "astro/config";

// https://astro.build/config
export default defineConfig({
  site: "https://girobai.github.io",
  base: "/lghp",
});
```

### `.github/workflows/deploy.yml`

```yml
# https://github.com/actions/starter-workflows/blob/main/pages/astro.yml
# https://docs.astro.build/en/getting-started/
name: Deploy to GitHub Pages

on:
  # Trigger the workflow every time you push to the `main` branch
  # Using a different branch name? Replace `main` with your branch’s name
  push:
    branches: ["main"]
    # Trigger the action only when files change in the folders defined here:
    # paths: ['src/**', 'public/**']

  # Allows you to run this workflow manually from the Actions tab on GitHub.
  workflow_dispatch:

# Allow this job to clone the repo and create a page deployment
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

env:
  BUILD_PATH: "." # default value when not using subfolders
  # BUILD_PATH: subfolder

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository using git
        uses: actions/checkout@v4

      - name: 🔎 Detect package manager
        id: detect-package-manager
        run: |
          if [ -f "${{ github.workspace }}/yarn.lock" ]; then
            echo "manager=yarn" >> $GITHUB_OUTPUT
            echo "command=install" >> $GITHUB_OUTPUT
            echo "runner=yarn" >> $GITHUB_OUTPUT
            echo "lockfile=yarn.lock" >> $GITHUB_OUTPUT
            exit 0
          elif [ -f "${{ github.workspace }}/package.json" ]; then
            echo "manager=npm" >> $GITHUB_OUTPUT
            echo "command=ci" >> $GITHUB_OUTPUT
            echo "runner=npx --no-install" >> $GITHUB_OUTPUT
            echo "lockfile=package-lock.json" >> $GITHUB_OUTPUT
            exit 0
          else
            echo "Unable to determine package manager"
            exit 1
          fi

      - run: echo " github.workspace ${{ github.workspace }}"
      - run: echo " steps.detect-package-manager.outputs.manager ${{ steps.detect-package-manager.outputs.manager }}"
      - run: echo " steps.detect-package-manager.outputs.lockfile ${{ steps.detect-package-manager.outputs.lockfile }}"
      - run: echo " steps.detect-package-manager.outputs.command ${{ steps.detect-package-manager.outputs.command }}"
      - run: echo " env.BUILD_PATH ${{ env.BUILD_PATH }}"
      - run: echo " steps.detect-package-manager.outputs.runner ${{ steps.detect-package-manager.outputs.runner }}"
      - run: echo " steps.pages.outputs.origin ${{ steps.pages.outputs.origin }}"
      - run: echo " steps.pages.outputs.base_path ${{ steps.pages.outputs.base_path }}"
      - run: echo " steps.pages.outputs.origin ${{ steps.pages.outputs.origin }}"
      - run: echo " steps.deployment.outputs.page_url ${{ steps.deployment.outputs.page_url }}"

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: ${{ steps.detect-package-manager.outputs.manager }}
          cache-dependency-path: ${{ env.BUILD_PATH }}/${{ steps.detect-package-manager.outputs.lockfile }}

      - name: 📟 Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: 📒 Install dependencies
        run: ${{ steps.detect-package-manager.outputs.manager }} ${{ steps.detect-package-manager.outputs.command }}
        working-directory: ${{ env.BUILD_PATH }}

      - name: 🔨 Build with Astro
        run: |
          ${{ steps.detect-package-manager.outputs.runner }} astro build \
            --site "${{ steps.pages.outputs.origin }}" \
            --base "${{ steps.pages.outputs.base_path }}"
        working-directory: ${{ env.BUILD_PATH }}

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ${{ env.BUILD_PATH }}/dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: build
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
