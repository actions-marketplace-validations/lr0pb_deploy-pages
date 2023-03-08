# 🛠️ Deploy app to GitHub Pages
[![Latest release version](https://img.shields.io/github/v/release/lr0pb/deploy-pages?color=g&label=Version&logo=github)](https://github.com/lr0pb/deploy-pages/releases)

Build your JavaScript/TypeScript apps with GitHub Actions and deploy it to GitHub Pages.
If you are new into GitHub Actions read [this quick start guide](https://docs.github.com/en/actions/quickstart) first

## Why
- 🚀 Support any framework and build tool
  > You specify your own npm script that build your app, so you can use any tools you want, i.e React, Vue, webpack, Rollup etc.

- ✅ Without creating additional commits
  > Deploy process provide build output directly to the GitHub Pages, so no need to create unnecessary commits

- 🧹 Don't store build output in your repo
  > Because build output goes directly to Pages, there is no need to store it in repo. Add output folder to `.gitignore` file

## Usage

Add following entry into your `jobs` list in workflow file
```yaml
deploy:
  runs-on: ubuntu-latest
  permissions:
    contents: read
    pages: write
    id-token: write
  environment:
    name: github-pages
    url: ${{ steps.deployment.outputs.url }}
  steps:
    - name: Deploy app to GitHub Pages
      uses: lr0pb/deploy-pages@v1
      id: deployment
      with:
        node-version: 18.x
        build-comand: 'build'
        output-dir: 'dist'
```

## Action inputs and outputs
### Inputs
It work like arguments for function and should set via `with` keyword in your `<step-name>` that use this Action. All inputs are optional.

| Name | Description | Default value |
| --- | --- | --- |
| `node-version` | Version of Node.js used to build your app | `lts/*`<br />(last LTS version) |
| `build-comand` | NPM script from your `package.json` used to build your application | `build`<br />(will call `npm run build`) |
| `output-dir` | Build output directory that will be published to GitHub Pages | `dist` |

### Outputs
Output can be accessed in your workflow via `steps.<id-of-your-step>.outputs.<property>`

| Name | Description |
| --- | --- |
| `url` | URL of hosted GitHub Pages environment, required to set as `github-pages` environment url |

## Ready-to-use workflow file
Full workflow file template for your deployment process:
```yaml
name: Deploy app

on:
  push:
    branches: ['main']
  workflow_dispatch:

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.url }}
    steps:
      - name: Deploy app to GitHub Pages
        uses: lr0pb/deploy-pages@v1
        id: deployment
        with:
          node-version: 18.x
          build-comand: 'build'
          output-dir: 'dist'
```
