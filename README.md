CKEditor 5 custom build
=======================

This is my own custom CKEditor 5 build.

Read more about [custom builds](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/development/custom-builds.html).

## Requirements

In order to start developing CKEditor 5 you will require:

* [Node.js](https://nodejs.org/en/) 6.9.0+
* npm 4+ (**note:** some npm 5+ versions were known to cause [problems](https://github.com/npm/npm/issues/16991), especially with deduplicating packages; upgrade npm when in doubt)
* [Git](https://git-scm.com/)

## Build anatomy

Every build contains the following files:

* `build/ckeditor.js` &ndash; The ready-to-use editor bundle, containing the editor and all plugins.
* `src/ckeditor.js` &ndash; The source entry point of the build. Based on it the `build/ckeditor.js` file is created by [webpack](https://webpack.js.org). It defines the editor creator, the list of plugins and the default configuration of a build.
* `webpack-config.js` &ndash; webpack configuration used to build the editor.

## Updating CKEditor

To make updating easier you may add the original build repository to your Git remotes:
```
git remote add upstream https://github.com/ckeditor/ckeditor5-build-classic.git
```

Since this is a fork of the official build, you can simply merge the changes that happened meanwhile in that build, using Git commands:
```
git fetch upstream
git merge upstream/stable
```

## Customizing the build

In order to customize a build you need to:

* Install missing dependencies.
* Update the `src/ckeditor.js` file.
* Update the build (the editor bundle in `build/`).

### Installing dependencies

First, you need to install dependencies which are already specified in the build's `package.json`:

```bash
npm install
```

Then, you can add missing dependencies (i.e. packages you want to add to the build). The easiest way to do so is by typing:

```bash
npm install --save-dev <package-name>
```

This will install the package and add it to `package.json`. You can also edit `package.json` manually.

Due to the non-deterministic way how npm installs packages, it is recommended to run `rm -rf node_modules && npm install` when in doubt. This will prevent some packages from getting installed more than once in `node_modules/` (which might lead to broken builds).

If you encounter the `ckeditor-duplicated-modules` error, try to delete your `node_modules` directory and your `package-lock.json` file.
If this doesn't help, see [error-ckeditor-duplicated-modules](https://ckeditor.com/docs/ckeditor5/latest/framework/guides/support/error-codes.html#error-ckeditor-duplicated-modules) on the CKEditor Documentation website.

### Rebuilding the bundle

After you changed the webpack entry file or updated some dependencies, it is time to rebuild the bundle. This will run a bundler (webpack) with a proper configuration (see `webpack.config.js`).

To do that, execute the following command:

```bash
npm run build
```

### Testing the bundle

You can validate whether your new build works by opening the `sample/index.html` file in a browser (via HTTP, not as a local file).

Install http-server globally:
```bash
npm install http-server -g
```

Then, in the project directory run:
```bash
http-server .
```

You can now open your browser and visit http://127.0.0.1:8080/sample/index.html
