# DEPRECATED
Yarn now publishes an official lockfile package at [@yarnpkg/lockfile](https://npm.im/@yarnpkg/lockfile)!

# yarn-lockfile
parse and/or write `yarn.lock` files

## So it turns out...

...that even though `yarn` doesn't follow the node "small modules" philosophy
in terms of making its guts available as packages in the registry, its
guts *are* modules. I've been wanting to make something interoperable with `yarn`
for ages but was put off by its 300 LOC [handwritten parser](https://github.com/yarnpkg/yarn/blob/master/src/lockfile/parse.js)
for its proprietary `yarn.lock` file format.

I don't know why they would invent a new file format when we have working JSON
and YAML libraries in every programming language known to man. But until such
time they release a library package for working with `yarn.lock` files, the parser
and stringifier can be accessed by directly requiring them from the `yarn` package:

```js
module.exports = {
  parse: require('yarn/lib/lockfile/parse.js').default,
  stringify: require('yarn/lib/lockfile/stringify.js').default
}
```

And that's all this module does. It works as you might expect:

```js
const fs = require('fs')
const lockfile = require('yarn-lockfile')

let file = fs.readFileSync('yarn.lock', 'utf8')
let json = lockfile.parse(file)

console.log(json)

let fileAgain = lockfile.stringify(json)

console.log(fileAgain)
```


## Wait, if it's that easy why did you publish this module?

Because the first thing I did when I wanted to parse a `yarn.lock` was search for
"yarn lockfile" on npmjs.org, and when I didn't find one and saw the parser in
yarn was handwritten, I gave up. It wasn't until a year later someone made me
realize you could do this. Now if someone searches npm for a yarn lockfile parser/stringifier, they will find one. Also, [small modules](http://dailyjs.com/post/small-modules-complexity-over-size)!


## License

Copyright 2017 William Hilton.
Licensed under [The Unlicense](http://unlicense.org/).
