# borschik-server2
[![Build Status](https://travis-ci.org/bem/borschik-server.png?branch=master)](https://travis-ci.org/bem/borschik-server)
[![NPM version](https://badge.fury.io/js/borschik-server.png)](http://badge.fury.io/js/borschik-server)
[![Dependency Status](https://david-dm.org/bem/borschik-server.png)](https://david-dm.org/bem/borschik-server)

An HTTP server to process JS and CSS files with [borschik](https://github.com/bem/borschik) on demand.

This server is **for development use only**.

## Default behavour
1. If file exists, respond with the file as is
2. Process files with `_` prefix only (can be redefined in `path-resolver`)
3. Minimize is disabled
4. Freeze is disabled

Some examples:
 1. Your request is `http://example.com/js/file.js`. `file.js` exists, so `borschik-server` reads the file
and writes it to the output as is.
 2. Your request is `http://example.com/js/_file.js`. `file.js` doesn't exist, but `borschik-server` removes the `_` prefix, reads file `file.js` and processes it with `borschik`.


## Installation
```
npm install -g borschik-server 
```

## Usage
Just run `borschik-server` and set up your webserver. If you want to use borschik-server as an init.d script,
follow [this template for Ubuntu](https://gist.github.com/peterhost/715255)

## Webserver configuration
You should set up your webserver (apache, lighttpd, nginx, etc.) to proxy http requests for static files to borschik-server.

Example of nginx configuration:
```
location ~* "\.(css|js)$" {
    # proxy all requests for css/js to borschik-server
    # use $uri (not $request_uri) to deal with rewrite
    proxy_pass http://127.0.0.1:8055$document_root$uri;
}
```


## How to extend with new technologies
You can create you own server with this code
```js
require('borschik-server').server({
    port: 8055,
    techResolver: require('../lib/tech-resolver')
});
```

This code references your own tech-resolver.
You can find an example in the [unit tests](./test/mock/custom-tech-resolver.js). In this example we add support for a new ".styl" technology.

You can redefine pathResolver as well. In our [example](./test/mock/custom-path-resolver.js) we define built files as `file.min.js`.

## License
[MIT](/MIT-LICENSE.txt)
