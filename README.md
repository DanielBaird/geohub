
# rickety copy of geohub

This is a shallow, poorly thought out copy of geohub to do a
proof-of-concept implementation of fetching a file from a github
repo using a specific sha.

The method used by original geohub is limited to files with a maximum
size of 1 Mb.  A blob data API exists to fetch files up to 100Mb in
size, and this modification is attempting to use that API.

Blame daniel@danielbaird.com for all problems.

Original geohub docs follow.

# geohub

> GeoJSON extractor for Github repos and gists.

[![npm version][npm-img]][npm-url]
[![build status][travis-image]][travis-url]

[npm-img]: https://img.shields.io/npm/v/geohub.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/geohub
[travis-image]: https://img.shields.io/travis/koopjs/geohub/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/koopjs/geohub

Extracts and parses GeoJSON files from Github repos and gists using the Github API.

## Install

```
npm install geohub
```

## Usage

```js
geohub = require('geohub')

var user = 'chelm'
var repo = 'grunt-geo'
var path = 'forks'

// Extract GeoJSON from a specific path in a repository
geohub.repo({
  user: user,  // username
  repo: repo,  // repository
  path: path,  // path to file or directory
  token: token // github access token
}, function (err, data) {
  if (err) throw err
  console.log(data)
  // => if path is a directory, data is an array of geojson objects
  // => if path is a filename (omit .geojson extension), data is a geojson object
})

// Omit the 'path' option to extract all GeoJSON files from a repository
geohub.repo({
  user: user,  // username
  repo: repo,  // repository
  token: token // github access token
}, function (err, data) {
  if (err) throw err
  console.log(data)
  // => data is an array of geojson objects
})

// You can also extract GeoJSON from a specific branch of a repo
geohub.repo({
  user: 'koopjs',           // username
  repo: 'geohub',           // repository
  branch: 'do-not-delete',  // branch name
  path: 'portland-parks',   // path to file or directory
  token: token              // github access token
}, function (err, data) {
  if (err) throw err
  console.log(data)
  // => data is a geojson object
})

// Check a file's SHA (useful for caching)
geohub.repoSha({
  user: user,  // username
  repo: repo,  // repository
  path: path,  // path to file
  token: token // github access token
}, function (err, sha) {
  if (err) throw err
  console.log(sha)
  // => SHA string
})

// Extract GeoJSON from a gist
geohub.gist({
  id: 6021269, // gist ID
  token: token // github access token
}, function (err, data) {
  if (err) throw err
  console.log(data)
  // => data is an array of geojson objects
})
```

## Test

Geohub uses [tape](https://github.com/substack/tape) for testing. It is recommended to create your own Github [access token](https://github.com/settings/tokens) for use during testing to ensure you will not hit Github API rate limits.

```
GITHUB_TOKEN=XXXXXX npm test
```

## License

[MIT](LICENSE)
