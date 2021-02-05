# General

This repository is a [MapStore Extensions](https://mapstore.readthedocs.io/en/latest/developer-guide/extensions/) repository to build cadastrapp.
It is based on https://github.com/geosolutions-it/MapStoreExtension with the customizations for geOrchestra, and it follows the same conventions.

It can be used also as a template to create new extensions for geOrchestra.

## Plugin configuration

Basically, the Urbanisme plugin allows the user to query for the NRU and the ADS data on the parcelle layer and also to print the data onto a pdf

### Local config

For example the plugin allows configuration of the following properties

- *helpUrl* - Plugin specific help url for more details on the extension, used by the help button
- *foncier* - activate/deactivate `foncier` functionalities
- *popup* - settings for popup

 ```javascript
 {
 "cfg": {
    "helpUrl": "https://github.com/georchestra/cadastrapp/wiki/Guide-Utilisateur",
    "foncier": true,
    "popup": {
      "minZoom": 14,
      "timeToShow": 1000
    }
  }
 }
 ```

## Quick Start

Clone the repository with the --recursive option to automatically clone submodules.

`git clone --recursive https://github.com/georchestra/mapstore2-cadastrapp`

Install NodeJS >= 12.16.1 , if needed, from [here](https://nodejs.org/en/download/releases/).

You can start the development application locally:

`npm install`

`npm start`

The application runs at `http://localhost:8081` afterwards. You will see, opening a map, the sample plugin on top of the map.

### Running geOrchestra

#### Backend Dev Setup

You can run this application and refer to a running back-end of geOrchestra by configuring `proxyConfig.js` in the root of the project.
You can configure this to point to your running instance of geOrchestra, with cadastrapp installed.

#### Proxy

If you will try to do requests to absolute URLs, you may be redirected to use the proxy. (the request will be transformed in something like `/mapstore/proxy?url=...`).
Make sure that this entry point(s) (configured in `proxyConfig.json`) are able to resolve the URL passed as parameter.
If supported, you can add the URL to `useCors` entry in `localConfig.json` (see mapstore documentation).

#### Authentication

If you need to login, you can run geOrchestra locally and use the header extension to fake the login (see [Dev documentation of GeOrchestra](https://docs.georchestra.geo-solutions.it/en/latest/developer/index.html#mocking-security)). When you will try to login from the login menu, you will be logged in as the user indicated in the headers.

### Build Extension

To build the extension you should run

- `npm run ext:build`

This will create a zip with the name of your extension in `dist` directory.

### Test Module

The current project contains the plugin on its own. In a production environment the extension will be loaded dynamically from the MapStore back-end.
You can simulate in dev-mode this condition by:

Commenting `js/app.js` the lines indicated in `js/app.jsx`, that allow to load the plugin in the main app.

```javascript
// Import plugin directly in application. Comment the 3 lines below to test the extension live.
const extensions = require('./extensions').default;
plugins.plugins = { ...plugins.plugins, ...extensions };
ConfigUtils.setConfigProp('translationsPath', ['./MapStore2/web/client/translations', './assets/translations']);
// end of lines to comment
```

- run, in 2 different console the following commands:
  - `npm run ext:start`
  - `npm run ext:startapp`

This will run webpack dev server on port 8081 with MapStore, simulating the `extensions.json`, and will run on port 8082 the effective modules to load.

## Dev Hints

Here a list of hints to develop your extension:

- In order to keep your changes as much self contained as possible we suggest to put all your code (and assets) in `js/extension/`. (Put css in `js/extension/assets/`, etc...)
- Use the `@mapstore` alias to refer to MapStore components. This helps your code to be compatible with future enhancements when mapstore will be published as a separated package, that can be shared

## Developing your own extension starting from this repo

This is basically a repository for a MapStore Extension. All the code of the extension is under `js/extension` directory. You can replace the `plugins/Extension.js` with your own file. and configure the project to develop your own application.
See [the dedicated section of the Readme of MapStore Extension for details](https://github.com/geosolutions-it/MapStoreExtension/blob/master/README.md#start-creating-your-own-extension)
