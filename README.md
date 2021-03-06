# Po.et JS

[![Build Status](https://travis-ci.org/poetapp/poet-js.svg?branch=master)](https://travis-ci.org/poetapp/poet-js) [![Greenkeeper badge](https://badges.greenkeeper.io/poetapp/poet-js.svg)](https://greenkeeper.io/) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)


Po.et JS is an small library that provides methods to easily create and sign Po.et Claims. 

### Installation
```
npm i @po.et/poet-js
```

### Usage

The main function you'll be using is `createClaim`:
```ts
import { Claim, ClaimType, createClaim } from '@po.et/poet-js' 

const workAttributes = {
  name: 'The Raven',
  author: 'Edgar Allan Poe',
  tags: 'poem',
  dateCreated: '',
  datePublished: '1845-01-29T03:00:00.000Z',
  content: 'Once upon a midnight dreary...'
}
const privateKey = ''
const claim = createClaim(
  privateKey,
  ClaimType.Work,
  workAttributes
)
```
Once this claim is created, you can publish it to a Po.et Node:
```ts
const response = await fetch(poetNodeUrl + '/works/', {
  method: 'POST',
  headers: {
	'Accept': 'application/json',
	'Content-Type': 'application/json'
  },
  body: JSON.stringify(claim)
})
```
Notice you don't need to wait for the server's response to know the claim's Id. You don't even need to publish it! `claim.id` is readily available right after calling `createClaim`.

## Contributing

### Compiling
Run `npm run build` to compile the source. This will run TypeScript on the source files and place the output in `dist/ts`, and then it'll run Babel and place the output in `dist/babel`.

Currently, we're only using Babel to support [absolute import paths](https://github.com/tleunen/babel-plugin-module-resolver) in the unit tests. 
> Absolute paths are only used in the tests. This is due to how Typescript and Babel process absolute paths: on build, Typescript transforms the .ts files into .js and places type definitions in .d.ts files. Babel, with the module-resolver plugin, will then transform the absolute paths in these .js files into relative paths, but will leave the .d.ts unchanged — which still have absolute paths. This causes issues with clients of the library that want to use these typescript definitions.    

### Tests
Run all tests with `npm test`.


### Coverage
Coverage is generated with [Istanbul](https://github.com/istanbuljs/nyc) whenever tests are run.
A more complete report can be generated by running `npm run coverage`.

This area needs some reviewing — the reports look a bit off sometimes.

Also, we'll want the Travis job to check that coverage doesn't go down with pull requests. 

### Branches and Pull Requests
The master branch is blocked - no one can commit to it directly. To contribute changes, branch off from master and make a PR back to it. 

TravisCI will run all tests automatically for all pull requests submitted.

### Code Style
Please run `npm run lint`. The linting configuration still needs some tweaking, and it'll be added to Travis in the future.

