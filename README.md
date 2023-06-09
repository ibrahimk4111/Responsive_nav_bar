# Responsive navigation bar with vite and reactJs

### TailwindCSS installation in vite Project:
#### Step 1:
`
npm install -D tailwindcss postcss autoprefixer
`
#### Step 2:
` npx tailwindcss init -p `
#### Step 3:
```export default {
    content: [
        "./index.html",
        "./src/**/*.{js,ts,jsx,tsx}",
        ],
        ........
}
```

## Follow the steps below on how to deploy a vite react app:

#### 01. Setup base on vite.config
`base: "/[REPO_NAME]/"`

#### 02. Create ./github/workflows/deploy.yml and add the code bellow
```
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

#### 03. Push
```
git add . 
git commit -m "add: deploy workflow" 
git push
```
#### 04. Active workflow
```
Settings / Actions / General / Workflow permissions / Read and Write permissions 
Actions / failed deploy / re-run-job failed jobs 
Pages / gh-pages / save
```