# lab-sandbox
Self pace labs or exercise in learning about features in GH

# Participate

In order for you to be able to work on this repo, you must branch first from main. There is a branch protection rule created with minimal checks so that you get used to working with a branch-protected repo.

1. create a new branch
1. Perform changes on the new branch
1. Perform a Pull Request from your new branch to the main branch

## Where are these labs

Why create these things? There is a wealth of material already out there to guide you.

Start Here:
https://docs.github.com/en/actions/guides

You can always use your trusty Bing Copilot to create sample workflows for you. 

Example prompt:
`Create a github workflow that has a build job, and deploy jobs for mona-dev, mona-test, and mona-prod environments, the build job will publish artifacts, and the deploy jobs will pull the artifacts. The deploy jobs are dependant on the build job in the workflow. Use the latest versions of github actions.`

You might get:

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

env:
  MONA_DEV: mona-dev
  MONA_TEST: mona-test
  MONA_PROD: mona-prod

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 14

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Archive build artifacts
        uses: actions/upload-artifact@v2
        with:
          name: build-artifacts
          path: build

  deploy:
    needs: build
    strategy:
      matrix:
        environment: [${{ env.MONA_DEV }}, ${{ env.MONA_TEST }}, ${{ env.MONA_PROD }}]
    runs-on: ubuntu-latest
    steps:
      - name: Download build artifacts
        uses: actions/download-artifact@v2
        with:
          name: build-artifacts
          path: build

      - name: Deploy to ${{ matrix.environment }}
        run: echo "Deploying to ${{ matrix.environment }}"


```