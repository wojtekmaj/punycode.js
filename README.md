# Punycode.js [![Build status](https://travis-ci.org/bestiejs/punycode.js.png?branch=master)](https://travis-ci.org/bestiejs/punycode.js) [![Dependency status](https://gemnasium.com/bestiejs/punycode.js.png)](https://gemnasium.com/bestiejs/punycode.js)

A robust Punycode converter that fully complies to [RFC 3492](http://tools.ietf.org/html/rfc3492) and [RFC 5891](http://tools.ietf.org/html/rfc5891), and works on nearly all JavaScript platforms.

This JavaScript library is the result of comparing, optimizing and documenting different open-source implementations of the Punycode algorithm:

* [The C example code from RFC 3492](http://tools.ietf.org/html/rfc3492#appendix-C)
* [`punycode.c` by _Markus W. Scherer_ (IBM)](http://opensource.apple.com/source/ICU/ICU-400.42/icuSources/common/punycode.c)
* [`punycode.c` by _Ben Noordhuis_](https://github.com/bnoordhuis/punycode/blob/master/punycode.c)
* [JavaScript implementation by _some_](http://stackoverflow.com/questions/183485/can-anyone-recommend-a-good-free-javascript-for-punycode-to-unicode-conversion/301287#301287)
* [`punycode.js` by _Ben Noordhuis_](https://github.com/joyent/node/blob/426298c8c1c0d5b5224ac3658c41e7c2a3fe9377/lib/punycode.js) (note: [not fully compliant](https://github.com/joyent/node/issues/2072))

This project is [bundled](https://github.com/joyent/node/blob/master/lib/punycode.js) with [Node.js v0.6.2+](https://github.com/joyent/node/compare/975f1930b1...61e796decc).

## Installation

In a browser:

~~~html
<script src="punycode.js"></script>
~~~

Via [npm](http://npmjs.org/) (only required for Node.js releases older than v0.6.2):

~~~bash
npm install punycode
~~~

In [Narwhal](http://narwhaljs.org/), [Node.js](http://nodejs.org/), and [RingoJS](http://ringojs.org/):

~~~js
var punycode = require('punycode');
~~~

In [Rhino](http://www.mozilla.org/rhino/):

~~~js
load('punycode.js');
~~~

Using an AMD loader like [RequireJS](http://requirejs.org/):

~~~js
require(
  {
    'paths': {
      'punycode': 'path/to/punycode'
    }
  },
  ['punycode'],
  function(punycode) {
    console.log(punycode);
  }
);
~~~

## API

### `punycode.decode(string)`

Converts a Punycode string of ASCII symbols to a string of Unicode symbols.

```js
// decode domain name parts
punycode.decode('maana-pta'); // 'mañana'
punycode.decode('--dqo34k'); // '☃-⌘'
```

### `punycode.encode(string)`

Converts a string of Unicode symbols to a Punycode string of ASCII symbols.

```js
// encode domain name parts
punycode.encode('mañana'); // 'maana-pta'
punycode.encode('☃-⌘'); // '--dqo34k'
```

### `punycode.toUnicode(domain)`

Converts a Punycode string representing a domain name to Unicode. Only the Punycoded parts of the domain name will be converted, i.e. it doesn’t matter if you call it on a string that has already been converted to Unicode.

```js
// decode domain names
punycode.toUnicode('xn--maana-pta.com'); // 'mañana.com'
punycode.toUnicode('xn----dqo34k.com'); // '☃-⌘.com'
```

### `punycode.toASCII(domain)`

Converts a Unicode string representing a domain name to Punycode. Only the non-ASCII parts of the domain name will be converted, i.e. it doesn’t matter if you call it with a domain that's already in ASCII.

```js
// encode domain names
punycode.toASCII('mañana.com'); // 'xn--maana-pta.com'
punycode.toASCII('☃-⌘.com'); // 'xn----dqo34k.com'
```

### `punycode.ucs2`

#### `punycode.ucs2.decode(string)`

Creates an array containing the numeric code point values of each Unicode symbol in the string. While [JavaScript uses UCS-2 internally](http://mathiasbynens.be/notes/javascript-encoding), this function will convert a pair of surrogate halves (each of which UCS-2 exposes as separate characters) into a single code point, matching UTF-16.

```js
punycode.ucs2.decode('abc'); // [0x61, 0x62, 0x63]
// surrogate pair for U+1D306 TETRAGRAM FOR CENTRE:
punycode.ucs2.decode('\uD834\uDF06'); // [0x1D306]
```

#### `punycode.ucs2.encode(codePoints)`

Creates a string based on an array of numeric code point values.

```js
punycode.ucs2.encode([0x61, 0x62, 0x63]); // 'abc'
punycode.ucs2.encode([0x1D306]); // '\uD834\uDF06'
```

### `punycode.version`

A string representing the current Punycode.js version number.

[Full API documentation is available.](https://github.com/bestiejs/punycode.js/tree/master/docs#readme)

## Unit tests & code coverage

After cloning this repository, run `npm install --dev` to install the dependencies needed for Punycode.js development and testing. You may want to install Istanbul _globally_ using `npm install istanbul -g`.

Once that’s done, you can run the unit tests in Node using `npm test` or `node tests/tests.js`. To run the tests in Rhino, Ringo, Narwhal, and web browsers as well, use `grunt test`.

To generate the code coverage report, use `grunt cover`.

Feel free to fork if you see possible improvements!

## Author

| [![twitter/mathias](http://gravatar.com/avatar/24e08a9ea84deb17ae121074d0f17125?s=70)](http://twitter.com/mathias "Follow @mathias on Twitter") |
|---|
| [Mathias Bynens](http://mathiasbynens.be/) |

## Contributors

| [![twitter/jdalton](http://gravatar.com/avatar/299a3d891ff1920b69c364d061007043?s=70)](http://twitter.com/jdalton "Follow @jdalton on Twitter") |
|---|
| [John-David Dalton](http://allyoucanleet.com/) |

## License

Punycode.js is dual licensed under the [MIT](http://mths.be/mit) and [GPL](http://mths.be/gpl) licenses.
