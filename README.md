# GeenenAuth Hapi Plugin

[![Build Status](https://travis-ci.org/GlennGeenen/geenenauth.svg?branch=master)](https://travis-ci.org/GlennGeenen/geenenauth)

This is a Hapi plugin wrapper around hapi-auth-jwt2 with a specific setup as used by GeenenTijd. Compatible with Glue.

## Install

```
npm i geenenauth --save
```

## Register

```
server.register({
  register: require('geenenauth'),
  options: {
    secret: 'MyJWTSecret',
    issuer: 'MyIssuer', // Default GeenenTijd
    audience: 'MyAudience',
    userRoles: [], // Default ['user', 'editor']
    adminRoles: [], // Default ['admin', 'superadmin']
  }
});
```
## Strategies

User Authentication:
```
server.route({
  method: 'GET',
  path: '/me',
  config: {
    auth: {
      strategy: 'jwtUser'
    }
  },
  handler: function (request, reply) {
  }
});
```
Admin Authentication:
```
server.route({
  method: 'GET',
  path: '/me',
  config: {
    auth: {
      strategy: 'jwtAdmin'
    }
  },
  handler: function (request, reply) {
  }
});
```

## isAdmin(user)

The plugin exposes an isAdmin method.

```
function (request, reply) {

  if (request.server.methods.isAdmin(request.auth.credentials)) {

    // User is admin

  }

}
```

## getToken(user, [options,] callback)

Options: [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)

- expiresIn
- notBefore
- noTimestamp

Example of settings a token that never expires (not recommended):

```
function (request, reply) {

  const user = {
    userid: 'c0cb1883-e8c6-4efa-8561-4ad4f4c14518',
    username: 'glenn',
    role: 'user'
  };

  request.server.methods.getToken(user, {
    expiresIn: undefined
  } (err, token) => {

    // We have token here
  });
}
```
