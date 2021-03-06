Bitmask Javascript UI
=================================================================

Here lies the user interface for Bitmask, written in Javascript.

quick start:

    sudo apt install nodejs npm nodejs-legacy
    npm install       # installs development dependencies in "node_modules"
    npm run watch     # continually rebuilds source .js into "pydist"

build for deployment:

    npm install
    npm run build:production

After 'build', 'build:production', or 'watch' is run, everything needed for
Bitmask JS is contained in the 'pydist' directory. No additional files are
needed. Open the pydist/bitmask_js/public/index.html file in a browser or web
widget.

However! Because of the single origin policy of browsers, you will need to
open public/index.html through the webserver included with bitmaskd (e.g.
http://localhost:7070)

In order for this JS app to be loaded by bitmask, it must be packaged as a
python package and installed in the virtualenv:

    source path-to-virtualenv/bin/activate
    make dev-install  # builds and installs JS app as python package
    pkill bitmaskd    # make sure bitmaskd is not already running
    bitmaskd          # launch backend
    npm run open      # opens http://localhost:7070/ in a browser
    npm run watch     # rebuild JS whenever source file is changed.

In order to package for distribution:

    make dist-build

NOTE: If you make changes to the asset files, like add or modify an image, you
      will need to stop then rerun `npm run watch` for the changes to take
      effect.

Development Dependencies
-----------------------------------------------------------------

This application has no "runtime" dependencies: all the javascript needed is
bundled and included. However, there are many development dependencies.
Run `npm ls` for a full list.

**npm**

Package management, controlled by the package.json file.

**webpack**

Asset bundling and transformation. It takes all your javascript and CSS and
bundles it into one (or more) files, handling support for 'require' and scope
separation.

loaders & plugins:

* babel-loader: use Babel with Webpack.

* style-loader/css-loader: standard css loader.

* less-loader: allows use the less stylesheet.

* copy-webpack-plugin: allows us to specify what files to copy using Webpack.

* extract-text-webpack-plugin: allows you to split js and css into separate
  files.

**babel**

Babel is used to compile javascript into javascript, transforming along the
way. We have enabled these plugins:

* babel-presets-react: Adds support for React features, such as JSX (html
  inlined in js files).

* babel-presets-es2015: Allows the use of modern ES6 javascript.

* babel-presets-stage-0: Allows the use of some ES7 proposals, even though
  these are not standardized yet. Makes classes nicer.

* babel-polyfill: This is not part of the babel transpiling, but is distributed
  by babel. This polyfill will give you a full ES2015 environment even if the
  browser is missing some javascript features. We include this in the 'entry'
  option of the webpack config. https://babeljs.io/docs/usage/polyfill/

**react**

React is an efficient way to generate HTML views with Javascript. It allows you
to create interactive UIs without ever modifying the DOM by using "one way"
data binding. This greatly simplifies the code and reduces errors.

**bootstrap**

The world's most popular CSS styles for UI elements. The npm package includes
both pre-compiled css and less stylesheets. Even though Semantic UI is better,
Bootstrap components for React are much more stable, I have found, and also are
easy to theme.

To integrate Bootstrap with React:

* react-bootstrap: React components that use Bootstrap styles. These component
  include all the needed javascript and don't require JQuery, although they do
  require that the bootstrap CSS is loaded independently.

**zxcvbn**

A password strength checker that doesn't suck, but which is big. This JS is
only loaded when we think we are about to need it.


