# Server

![Build Status](https://github.com/janis-commerce/server/workflows/Build%20Status/badge.svg)
[![npm version](https://badge.fury.io/js/%40janiscommerce%2Fserver.svg)](https://www.npmjs.com/package/@janiscommerce/server)

A package for managing a Server with HTTP Server. This package implements [express](https://www.npmjs.com/package/express).

## Installation

```
npm install @janiscommerce/server
```

## Usage

```js
const Server = require('@janiscommerce/server');

const AuthClass = require('./src/controllers/authorization');

new Server({
	validateSchemas: true,
	cors: {
		origin: true,
		credentials: true
	},
	authorizationClass: AuthClass
});
```

### options

Use this options to configurate the server instance

| Option | Type | Description | Default value | Extras |
|--------|------|-------------|------------|---------------|
| validateSchemas | boolean | Indicates if the server has to validate the api schemas using the package [@janiscommerce/schema-validator](https://www.npmjs.com/package/@janiscommerce/schema-validator) | false | |
| cors | boolean|object | Used to implement APIs [Cors](https://www.npmjs.com/package/cors) configuration as custom props. If option is `true` it will use the default CORS configs.| [See below](#cors-default) | |
| authorizationClass | object | A class that implements the authorization logic | | [See below](#autorization-class) |

#### CORS Default:

```js
{
	origins: ['*'],
	headers: [
		'authorization',
		'content-type',
		'janis-api-key',
		'janis-api-secret',
		'janis-client',
		'janis-service',
		'janis-entity',
		'x-api-key',
		'x-janis-page',
		'x-janis-page-size'
	],
	allowCredentials: true,
	maxAge: 600
}
```

## Autorization Class

### validateRequest(request, response)
This method is required, and should have the logic of your authorizacion. Receives by parameters the request and response objects

```js
'use strict';

const SchemaValidator = require('@janiscommerce/schema-validator');

const authorizationHeaders = ['api-key', 'api-secret'];

module.exports = class Authorization {

	async validateRequest(request, response) {

		{ headers: this.headers} = request;

		if(!this.requestHasAuthorizationHeaders(authorizationHeaders))
			throw new Error('Request has not the api keys headers or are empty');

		return this.validateApiSecuritySchemas();
	}

	requestHasAuthorizationHeaders(authorizationHeaders) {

		for(const authorizationHeader of authorizationHeaders) {
			if(!this.headers[authorizationHeader])
				return false;
		}

		return true;
	}

	validateApiSecuritySchemas() {

		// validation code
	}

};

```