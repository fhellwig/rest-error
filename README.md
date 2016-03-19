# RestError

Provides the RestError class and associated factory methods.

## Usage

```
$ npm install --save resterror
```

```javascript
const RestError = require('resterror');

function sqrt(val) {
	if (val < 0) {
	    throw RestError.badRequest('Value %d cannot be negative.', val);
	} else {
	    return Math.sqrt(val);
    }
}

let err = RestError.internalServerError('Cannot connect to the database.');

console.log(err.toString());
// Error: 500 (Internal Server Error) Cannot connect to the database.
```

The `badRequest` method and `internalServerError` method are factory methods. No `new` keyword is required. There is one factory method for each of the 400- and 500-series errors.

If you want to call the constructor yourself, you can:

```javascript
function sqrt(val) {
	if (val < 0) {
	    throw new RestError(400, 'Bad Request', util.format('Value %d cannot be negative.', val));
	} else {
	    return Math.sqrt(val);
    }
}
```

As you can see, using the factory method...

- is more readable,
- does not require the `new` keyword,
- includes the descriptive text, and
- handles `util.format` arguments.

You can also pass in an `Error` object...

```javascript
let err = new Error('That record already exists.');

let conflict = RestError.conflict(err);

console.log(conflict.toString());
// Error: 409 (Conflict) That record already exists.
```

This is useful when wrapping a library error into an error for your REST API.
