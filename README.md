# Punycode.ts

Port of [Punycode.js](https://github.com/bestiejs/punycode.js)

A robust Punycode converter that fully complies to [RFC 3492](https://tools.ietf.org/html/rfc3492) and [RFC 5891](https://tools.ietf.org/html/rfc5891).

## Installation

```js
import { ucs2decode, ucs2encode, decode, encode, toASCII, toUnicode } from './punycode.ts';
```

## API

### `decode(string)`

Converts a Punycode string of ASCII symbols to a string of Unicode symbols.

```js
// decode domain name parts
decode('maana-pta'); // 'mañana'
decode('--dqo34k'); // '☃-⌘'
```

### `encode(string)`

Converts a string of Unicode symbols to a Punycode string of ASCII symbols.

```js
// encode domain name parts
encode('mañana'); // 'maana-pta'
encode('☃-⌘'); // '--dqo34k'
```

### `toUnicode(input)`

Converts a Punycode string representing a domain name or an email address to Unicode. Only the Punycoded parts of the input will be converted, i.e. it doesn’t matter if you call it on a string that has already been converted to Unicode.

```js
// decode domain names
toUnicode('xn--maana-pta.com');
// → 'mañana.com'
toUnicode('xn----dqo34k.com');
// → '☃-⌘.com'

// decode email addresses
toUnicode('джумла@xn--p-8sbkgc5ag7bhce.xn--ba-lmcq');
// → 'джумла@джpумлатест.bрфa'
```

### `toASCII(input)`

Converts a lowercased Unicode string representing a domain name or an email address to Punycode. Only the non-ASCII parts of the input will be converted, i.e. it doesn’t matter if you call it with a domain that’s already in ASCII.

```js
// encode domain names
toASCII('mañana.com');
// → 'xn--maana-pta.com'
toASCII('☃-⌘.com');
// → 'xn----dqo34k.com'

// encode email addresses
toASCII('джумла@джpумлатест.bрфa');
// → 'джумла@xn--p-8sbkgc5ag7bhce.xn--ba-lmcq'
```

### `ucs2decode(string)`

Creates an array containing the numeric code point values of each Unicode symbol in the string. While [JavaScript uses UCS-2 internally](https://mathiasbynens.be/notes/javascript-encoding), this function will convert a pair of surrogate halves (each of which UCS-2 exposes as separate characters) into a single code point, matching UTF-16.

```js
ucs2decode('abc');
// → [0x61, 0x62, 0x63]
// surrogate pair for U+1D306 TETRAGRAM FOR CENTRE:
ucs2decode('\uD834\uDF06');
// → [0x1D306]
```

#### `ucs2encode(codePoints)`

Creates a string based on an array of numeric code point values.

```js
ucs2encode([0x61, 0x62, 0x63]);
// → 'abc'
ucs2encode([0x1D306]);
// → '\uD834\uDF06'
```

## License

Punycode.ts is available under the MIT license.
