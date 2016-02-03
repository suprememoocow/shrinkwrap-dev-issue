## Demonstration of `npm shrinkwrap` issue

This is why you should never run `npm shrinkwrap` when you've got dev-dependencies installed:

```
~/code/opensource/shrinkwrap-dev-issue $ rm -rf node_modules/ npm-shrinkwrap.json
~/code/opensource/shrinkwrap-dev-issue $ npm i
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
~/code/opensource/shrinkwrap-dev-issue $ npm shrinkwrap
npm info it worked if it ends with ok
npm info using npm@2.14.11
npm info using node@v0.10.33
npm WARN shrinkwrap Excluding devDependency: inherits { glob: '^6.0.4' }
wrote npm-shrinkwrap.json
npm info ok
~/code/opensource/shrinkwrap-dev-issue $ grep inherits npm-shrinkwrap.json
~/code/opensource/shrinkwrap-dev-issue $
```
