## Transform JSX/ES 2015 during development using Babel-cli and npm scripts

This setup involves using the Babel-cli, Babel [presets/plugins](http://babeljs.io/docs/plugins/), and npm scripts to transform JSX/ES 2015 to ES5 code during development.

We'll create this setup in seven steps. Or, [clone/download](https://github.com/codylindley/simple-react-babel-setup-4-webpages) this code then run `npm install` and skip to the last step.

### Step 1: Verify Node.js and npm then install global packages

In this step make sure you have installed or have the most recent stable version of [Node.js and npm](https://nodejs.org/en/). Then run the following command to install [browser-sync](https://www.browsersync.io/).

```
> npm install browser-sync -g
```

You may need to use "sudo" to install the package globally.

### Step 2: Create project directory and files

On your local file system create a directory with the following sub-directories and files.

```
├── index.html
├── jsBuild
├── jsSrc
│    └── app.js
└── package.json
```

Open the `package.json` file and place the following empty JSON object inside of it:

```
{}
```

### Step 3: Install npm devdependencies and dependencies

Open a command prompt from the root of the directory you created in step 2. Then run the following npm commands

```
> npm install babel-cli babel-preset-es2015 babel-preset-react browser-sync --save-dev
```

and

```
> npm install react react-dom --save
```

Running these two commands will install the necessary npm packages for this setup. The project directory `node_modules` folder should now contain the following npm packages:

```
├── index.html
├── jsBuild
├── jsSrc
│   └── app.js
├── node_modules
│   ├── babel-cli
│   ├── babel-preset-es2015
│   ├── babel-preset-react
│   ├── browser-sync
│   ├── react
│   └── react-dom
└── package.json
```

### Step 4: Configure Babel & npm scripts

Open the `package.json` file which should look something like this:

```
{
  "devDependencies": {
    "babel-cli": "^6.8.0",
    "babel-preset-es2015": "^6.6.0",
    "babel-preset-react": "^6.5.0",
    "browser-sync": "^2.12.5"
  },
  "dependencies": {
    "react": "^15.0.2",
    "react-dom": "^15.0.2"
  }
}
```

Add the following Babel and scripts configurations to the `package.json` file.

```
{
  "scripts": {
    "babel": "babel jsSrc --out-dir jsBuild -w",
    "server": "browser-sync --port 4000 start --server --files \"**/*.html\" \"**/*.css\" \"jsBuild/**/*.js\" "
  },
  "babel": {
    "presets": [
      "es2015",
      "react"
    ],
    "sourceMaps": false
  },
  "devDependencies": {
    "babel-cli": "^6.8.0",
    "babel-preset-es2015": "^6.6.0",
    "babel-preset-react": "^6.5.0",
    "browser-sync": "^2.12.5"
  },
  "dependencies": {
    "react": "^15.0.2",
    "react-dom": "^15.0.2"
  }
}
```

These updates configure Babel with the presents we installed from npm and provides two `"scripts"` we can run using the npm cli.

### Step 5: Update index.html

Open the `index.html` file and copy the following HTML into the file:

```
<!DOCTYPE html>
<html>
    <head>
		<title>React via Babel</title>
        <script src="node_modules/react/dist/react.js"></script>
        <script src="node_modules/react-dom/dist/react-dom.js"></script>
    </head>
<body>
    <div id="app"></div>
	<script src="jsBuild/app.js"></script>
</body>
</html>
```

Note we are pulling `react.js` and `react-dom.js` from the `node_modules` directory.

### Step 6: Update app.js

Open the `app.js` file and copy the following JavaScript into the file:

```
const HelloMessage = React.createClass({
	render: function() {
		return <div>Hello {this.props.name}</div>;
	}
});

ReactDOM.render(<HelloMessage name="Johnn" />, document.getElementById('app'));
```

### Step 7: Run Babel and server

From the root of the setup directory open a command prompt and run the following npm command

```
> npm run babel
```

Next, open another new command prompt and run the following npm command

```
> npm run server
```

Both of these commands will continue to run while developing.

If you followed all the steps correctly Browser Sync should have open a browser running the `index.html` file and `app.js` file at [http://localhost:4000](http://localhost:4000). Both Babel and Browser Sync have been configured to re-run when changes are made.
