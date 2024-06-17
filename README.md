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

If any depracations during `npm install`, run the following commands:
```sh
# Update all packages to the latest version based on the version ranges specified in package.json
npm update

# Upgrade all packages to their latest versions, ignoring the version ranges specified in package.json
npm install -g npm-check-updates
ncu -u
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

#### *Compile and Minify for Production*

```sh
npm run build
```

#### *Run Deploy*

```sh
npm run deploy
```

#### *Setup Comment Section*

```sh
npm install --save vue-comment-grid
```


#### *Google Sheets and AppScript*
```sh
function doPost(e) {
	let sheetName = "Sheet1";
	let sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(sheetName);
	if (!sheet) {
		return ContentService.createTextOutput("Error: Sheet not found");
	}

	// Check if the parameter object is defined
	if (!e || !e.parameter) {
		return ContentService.createTextOutput("Error: No parameters provided");
	}

	let newRow = sheet.getLastRow() + 1;
	let rowData = [];

	rowData.push(new Date()); // Timestamp

	// Add default values or empty strings if the parameters are missing
	rowData.push(e.parameter.Name || ""); // Name
	rowData.push(e.parameter.AccompanyingInvitees || ""); // AccompanyingInvitees
	rowData.push(e.parameter.Guests || ""); // Guests
	rowData.push(e.parameter.Status || ""); // Status

	sheet.getRange(newRow, 1, 1, rowData.length).setValues([rowData]);

	return ContentService.createTextOutput('Thank you! Your RSVP has been sent.');
}
```

See [Vite Static Deploy Reference](https://vitejs.dev/guide/static-deploy.html).

Create a file called `.gitlab-ci.yml` in the root of your project with the content below. This will build and deploy your site whenever you make changes to your content:
```sh
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
