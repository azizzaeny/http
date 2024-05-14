## @zaeny/http 

[![npm version](https://img.shields.io/npm/v/@zaeny/http.svg)](https://www.npmjs.com/package/@zaeny/http)
![npm downloads](https://img.shields.io/npm/dm/@zaeny/http.svg)  


> Simple HTTP Server in node.js    

Translate incomin http request into data strcuture like ring in clojure  
Provide basic funcitonal programming utility  creating http server  

- [Geting Started](#getting-started)
- [Usage](#usage)
- [API](#api)
- [Related work](#related-work)

### Getting started 

```sh
npm i @zaeny/http
```

### Usage 
basic http server, with handler accept returning object `{status, headers, body}`
```js
var { createServer, startServer, response } = require('@zaeny/http');

var mainHandler = (req, res) => ({ status: 200, headers: {}, body: 'hello world'});
var server = server || startServer(
  createServer({ port: 8081, handler: (req, res) => mainHandler(req, res)})
);t
```

create simple routing

```js

var responseIndex = (req, res) => findFile('./index.html');
var getUser = (req, res) => response('fetch all  user');

var mainHandler = (req, res) => {
  let routes = {
    `GET /`: responseIndex,
    `GET /api/user`: getUser
  }
  let resolve = routes[`${req.method} ${req.path}`];
  if(resolve) return resolve(req, res);
  return notFound();
}

var server = server || startSever(
  createServer({ port: 8081, handler: (req, res) => mainHandler(req, res)})
);
```

usage with `@zaeny/clojure.core`

``` js
var {threadFirst, assoc} = require('@zaeny/clojure.core');

var info = (req, res) => response('hello world');

var server = threadFirst(
  { port: 8081, handler: info },
  createServer,
  startServer
);

```

basic routing

```js

var routes = () => ({
  'GET /api/audio/chapter/:id': 'handleGetAudioByChapter',
  'GET /asset/:id': 'serveAudioAssets'  
});

var resolve = {
  serveAudioAssets,
  handleGetAudioByChapter
}

var mainHandler = (req, res) => {
  let found = findRoutes(routes(), req);
  if(found && resolve[found]){
    return resolve[found](req, res);
  }
  return notFound('404');
}

var server = threadFirst(
  { port: 8081, handler: mainHandler },
  createServer,
  startServer
);

```

### API
```js
  createServer,
  startServer,
  stopServer,
  getServer,
  response,
  redirect,
  created,
  badRequest,
  notFound,
  status,
  header,
  headers,
  contentType,
  cors,
  isContentType,
  responseWrite,
  clientRequest,
  responseBuffer,
  responseWith,
  readFile,
  findFile,
  mimeType
```

### Related work
- [Composable](https://github.com/azizzaeny/composable/tree/main) - Collection of functions to solve programming problem

### Changes
 - [1.0.0] expose common api dealing with creating http server
 - [1.0.1] add `findRoutes` utility basic routing `GET /api/audio/:id`, params matching and stringify body request if json
 
