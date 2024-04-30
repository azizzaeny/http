## @zaeny/http 
Translate incomin http request into data strcuture like ring in clojure  

### Getting started 
(status: work in progress)  

### Usage 
basic http server, with handler accept returning object `{status, headers, body}`
```js

var mainHandler = (req, res) => ({ status: 200, headers: {}, body: 'hello world'});
var server = server || startSever(
  createServer({ port: 8081, handler: (req, res) => mainHandler(req, res)})
);
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
use in repl

`node` repl 

```js

var evaluate= (...args) => {
  let [vm=require('vm'), ctx=global, addCtx={console, require, module}] = args;
  return (res) => {
    let context = vm.createContext(ctx);
    return vm.runInContext(res, Object.assign(context, addCtx));
  }
}

var addDeps = (url) => fetch(url).then(res => res.text()).then(evaluate());

var deps = {
  http : "https://cdn.jsdelivr.net/gh/azizzaeny/http/dist/index.js", // to be loaded in  the repl as global vars
}

addDeps(deps.http);

```

live action: 
{todo: demo}

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

