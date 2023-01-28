# 🛠️ Deploy app to GitHub Pages
![Latest release version](https://img.shields.io/github/v/release/lr0pb/deploy-pages?color=g&label=version&logo=github)

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
Add permissions setting before `jobs` list in your workflow file
```yaml
# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write
```
And then add following entry into your `jobs` list
```yaml
<job-name>:
  runs-on: ubuntu-latest
  environment:
    name: github-pages
    url: ${{ steps.deploy.outputs.url }}
  steps:
    - name: Deploy app to GitHub Pages
      id: deploy
      uses: lr0pb/deploy-pages@v1
      with:
        node-version: 18.x
        build-comand: 'build'
        output-dir: 'dist'
```

## Action arguments
| Name | Description | Default value |
| --- | --- | --- |
| `node-version` | Version of Node.js used to build your app | `lts/*` (last LTS version) |
| `build-comand` | NPM script from your `package.json` used to build your application | `build` (will call `npm run build`) |
| `output-dir` | Build output directory that will be published to GitHub Pages | `dist` |

### Ready-to-use workflow file
Full YAML file template for your deploy workflow:
```yaml
name: Deploy app

on:
  push:
    branches: ['main']
  pull_request:
    branches: ['main']
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deploy.outputs.url }}
    steps:
      - name: Deploy app to GitHub Pages
        id: deploy
        uses: lr0pb/deploy-pages@v1
        with:
          node-version: 18.x
          build-comand: 'build'
          output-dir: 'dist'
```
