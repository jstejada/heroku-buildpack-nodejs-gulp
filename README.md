Heroku Buildpack for Node.js and gulp.js
========================================

Usage
-----

- Set your Heroku app's buildpack URL to `https://github.com/jstejada/heroku-buildpack-nodejs-gulp.git`. To be safe, you should really fork this and use your fork's URL.
- Run `heroku config:set
  BUILDPACK_URL=https://github.com/jstejada/heroku-buildpack-nodejs-gulp.git` to set custom buildpack
- Run `heroku config:set NODE_ENV=production` to set your environment to `production` (or any other name)
- Add a Gulp task called `heroku:production` that builds your app
  Supports `gulpfile.js` and `gulpfile.coffee`
- Install the dependenies for serving the app: `npm install gzippo express --save`
- Create a simple web server in the root called `server.js`:

```
var gzippo = require('gzippo');
var express = require('express');
var app = express();

app.use(express.logger('dev'));
app.use(gzippo.staticGzip("" + __dirname + "/build"));
app.listen(process.env.PORT || 5000);
```

- Add a single line `Procfile` to the root to serve the app via node:

```
web: node server.js
```

- Alternatively, if no Procfile is present in the root directory of your app,
  during the build process, Heroku will check for a scripts.start entry in your
  package.json file. If a start script entry is present, a default Procfile is
  generated automatically:

```
web: npm start
```

Credits
-------

Inspired by [Deploying a Yeoman/Angular app to Heroku](http://www.sitepoint.com/deploying-yeomanangular-app-heroku/).

Forked from [heroku-buildpack-nodejs-gulp](https://github.com/appstack/heroku-buildpack-nodejs-gulp).

Which was forked from [heroku-buildpack-nodejs](https://github.com/heroku/heroku-buildpack-nodejs).

Heavily based on [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt).
