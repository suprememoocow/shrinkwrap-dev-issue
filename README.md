## Demonstration of `npm shrinkwrap` issue

This is why you should never run `npm shrinkwrap` when you've got dev-dependencies installed:


### The problem

```shell
$ git clone git@github.com:suprememoocow/shrinkwrap-dev-issue.git
$ cd shrinkwrap-dev-issue
$ npm i
npm info using npm@2.14.11
npm info using node@v0.10.33
...
inherits@2.0.1 node_modules/inherits

glob@6.0.4 node_modules/glob
├── path-is-absolute@1.0.0
├── once@1.3.3 (wrappy@1.0.1)
├── inflight@1.0.4 (wrappy@1.0.1)
└── minimatch@3.0.0 (brace-expansion@1.1.2)
npm info ok
$ npm shrinkwrap
npm info it worked if it ends with ok
npm info using npm@2.14.11
npm info using node@v0.10.33
npm WARN shrinkwrap Excluding devDependency: inherits { glob: '^6.0.4' }
wrote npm-shrinkwrap.json
npm info ok
```

Now, simulate a production install in a new environment...

```shell
$ rm -rf node_modules
$ npm install --production
$ node index.js

module.js:340
    throw err;
          ^
Error: Cannot find module 'inherits'
    at Function.Module._resolveFilename (module.js:338:15)
    at Function.Module._load (module.js:280:25)
    at Module.require (module.js:364:17)
    at require (module.js:380:17)
    at Object.<anonymous> (/.../shrinkwrap-dev-issue/node_modules/glob/glob.js:46:16)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Module.require (module.js:364:17)
```

### The solution

Always do an npm shrinkwrap on a clean **production only** `node_modules` directory.

```shell
$ rm -rf node_modules/ npm-shrinkwrap.json
$ npm install --production
$ npm shrinkwrap
$ npm install # Now install dev-dependencies
```

Now, simulate a production install in a new environment...

```shell
$ rm -rf node_modules
$ npm install --production
$ node index.js
## Script runs correctly
```
