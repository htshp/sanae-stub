sanae-stub
====

> Simple api stub server.

usage
----

### Use in script

```sh
$ npm install --save sanae-stub
```

```js
/*
    REST like API sample.
    GET    /api/v2/users
    GET    /api/v2/users/:id
    POST   /api/v2/users?name&age
    DELETE /api/v2/users/:id
    PUT    /api/v2/users/:id?name&age
*/

const sanaeStub = require('sanae-stub');

// Define stub.
const stub = {
    'api/v2': {
        users: {
            GET: [
                { id: 0, name: taro,  age: 20 },
                { id: 1, name: alice, age: 25 }
            ],
            ':id': {
                GET: req => ({
                    status: 200,
                    body: `{ id: ${req.params.id}, name: taro, age: 20 }`,
                    headers: {
                        'Content-Type': 'application/json'
                    }
                }),
                DELETE: req => `Delete ID(${req.params.id}) user!`,
                PUT: req => `Update ${req.query.name}!`
            },
            POST: req => ({
                    status: 200,
                    body: `Register ${req.query.name}!`,
            })
        }
    }
};

// Listen http://localhost:3000
sanaeStub.createServer(stub).listen(3000);
```

### CLI

```sh
$ npm install -g sanae-stub
$ cat stub.json
    {
        "api/v2": {
            "users": {
                "GET": [],
                ":id": {
                    "GET": "{name: taro, age: 100 }"
                }
            }
        }
    }
$ sanae-stub stub.json
```

API
----

### Request

`req.method`: string

Request method name. 'GET' or 'POST', 'PUT', 'DELETE'.

`req.path`: string

`req.headers`: { 'headerName': 'value' }

`req.query`: { 'queryName': 'value' }

`req.params`: { 'paramName': 'value' }

`req.body`: string

### Response

```js
{
    status: 'statusCode',
    body: 'body',
    headers: 'headers'
}
```
