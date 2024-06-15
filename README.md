# Wedding Application

### CONFIGURATION

See [Vite Configuration Reference](https://vitejs.dev/config/).

See [Vue Firebase Authentication](https://learnvue.co/articles/vue-firebase-authentication).

### FRAMEWORKS AND LIBRARIES
- Vite and Vue
- Tailwind CSS
- Flowbite components
- Google apps script and Google spreadsheets
- fontAwesome
- expressjs API
- Firebase REST API and Authentication

### PROJECT SETUP

```sh
npm install
```

#### *Compile and Reload for Development*

```sh
npm run dev
```

#### *Install Firebase*
```sh
npm install firebase
```

```sh
firebase login
```

#### *Google Sheets and AppScript*
```sh
npm install googleapis
```

#### *Compile and Minify for Production*

```sh
npm run build
```

#### *Run Deploy*

```sh
npm run deploy
```

See [Vite Static Deploy Reference](https://vitejs.dev/guide/static-deploy.html).

Create a file called `.gitlab-ci.yml` in the root of your project with the content below. This will build and deploy your site whenever you make changes to your content:
```
image: node:16.5.0
pages:
  stage: deploy
  cache:
    key:
      files:
        - package-lock.json
      prefix: npm
    paths:
      - node_modules/
  script:
    - npm install
    - npm run build
    - cp -a dist/. public/
  artifacts:
    paths:
      - public
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
```

If any depracations during `npm install`, run the following commands:
```sh
# Update all packages to the latest version based on the version ranges specified in package.json
npm update

# Upgrade all packages to their latest versions, ignoring the version ranges specified in package.json
npm install -g npm-check-updates
ncu -u
npm install
```