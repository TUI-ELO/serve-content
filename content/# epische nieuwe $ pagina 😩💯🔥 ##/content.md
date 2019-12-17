The `**encodeURIComponent()**` function encodes a Uniform Resource Identifier (URI) component by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

The source for this interactive example is stored in a GitHub repository. If you'd like to contribute to the interactive examples project, please clone [https://github.com/mdn/interactive-examples](https://github.com/mdn/interactive-examples) and send us a pull request.

## Syntax

encodeURIComponent(str);

### Parameters

### Return value

A new string representing the provided string encoded as a Uniform Resource Identifier (URI) component.

## Description

`encodeURIComponent` escapes all characters **except**:

**`N`ot Escaped**:

    A-Z a-z 0-9 `-` `_` `.` `!` `~` `*` `'` `(` `)` 

`encodeURIComponent` differs from **`encodeURI`** as follows:

```
var set1 = ";,/?:@&=+$";  // Reserved Characters
var set2 = "-_.!~*'()";   // Unescaped Characters
var set3 = "#";           // Number Sign
var set4 = "ABC abc 123"; // Alphanumeric Characters + Space

console.log(encodeURI(set1)); // ;,/?:@&=+$
console.log(encodeURI(set2)); // -_.!~*'()
console.log(encodeURI(set3)); // #
console.log(encodeURI(set4)); // ABC%20abc%20123 (the space gets encoded as %20)

console.log(encodeURIComponent(set1)); // %3B%2C%2F%3F%3A%40%26%3D%2B%24
console.log(encodeURIComponent(set2)); // -_.!~*'()
console.log(encodeURIComponent(set3)); // %23
console.log(encodeURIComponent(set4)); // ABC%20abc%20123 (the space gets encoded as %20)


```

Note that a [`URIError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/URIError "The URIError object represents an error when a global URI handling function was used in a wrong way.") will be thrown if one attempts to encode a surrogate which is not part of a high-low pair, e.g.,

```
// high-low pair ok
console.log(encodeURIComponent('\uD800\uDFFF'));

// lone high surrogate throws "URIError: malformed URI sequence"
console.log(encodeURIComponent('\uD800'));

// lone low surrogate throws "URIError: malformed URI sequence"
console.log(encodeURIComponent('\uDFFF')); 

```

Use `encodeURIComponent` on user-entered fields from forms POST'd to the server. This will encode "&" symbols that may inadvertently be generated during data entry for special HTML entities or other characters that require encoding/decoding.

For example, if a user writes "Jack & Jill", the text may get encoded as "Jack &amp; Jill". Without `encodeURIComponent` the ampersand could be interpretted on the server as the start of a new field and jeopardize the integrity of the data.

For [`application/x-www-form-urlencoded`](http://www.whatwg.org/specs/web-apps/current-work/multipage/association-of-controls-and-forms.html#application/x-www-form-urlencoded-encoding-algorithm), spaces are to be replaced by '+', so one may wish to follow a `encodeURIComponent` replacement with an additional replacement of "%20" with "+".

To be more stringent in adhering to [RFC 3986](http://tools.ietf.org/html/rfc3986) (which reserves !, ', (, ), and \*), even though these characters have no formalized URI delimiting uses, the following can be safely used:

```
function fixedEncodeURIComponent(str) {
  return encodeURIComponent(str).replace(/[!'()*]/g, function(c) {
    return '%' + c.charCodeAt(0).toString(16);
  });
}

```

## Examples

The following example provides the special encoding required within UTF-8 `Content-Disposition` and `Link` server response header parameters (e.g., UTF-8 filenames):

```
var fileName = 'my file(2).txt';
var header = "Content-Disposition: attachment; filename*=UTF-8''" 
             + encodeRFC5987ValueChars(fileName);

console.log(header); 
// logs "Content-Disposition: attachment; filename*=UTF-8''my%20file%282%29.txt"


function encodeRFC5987ValueChars(str) {
    return encodeURIComponent(str).
        // Note that although RFC3986 reserves "!", RFC5987 does not,
        // so we do not need to escape it
        replace(/['()]/g, escape). // i.e., %27 %28 %29
        replace(/\*/g, '%2A').
            // The following are not required for percent-encoding per RFC5987, 
            // so we can allow for a little better readability over the wire: |`^
            replace(/%(?:7C|60|5E)/g, unescape);
}

// here is an alternative to the above function
function encodeRFC5987ValueChars2(str) {
  return encodeURIComponent(str).
    // Note that although RFC3986 reserves "!", RFC5987 does not,
    // so we do not need to escape it
    replace(/['()*]/g, c => "%" + c.charCodeAt(0).toString(16)). // i.e., %27 %28 %29 %2a (Note that valid encoding of "*" is %2A
                                                                 // which necessitates calling toUpperCase() to properly encode)
    // The following are not required for percent-encoding per RFC5987,
    // so we can allow for a little better readability over the wire: |`^
    replace(/%(7C|60|5E)/g, (str, hex) => String.fromCharCode(parseInt(hex, 16)));
}
```

## Specifications

| Specification | Status | Comment |
| --- | --- | --- |
| [ECMAScript 3rd Edition (ECMA-262)](https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-262,%203rd%20edition,%20December%201999.pdf "The 'ECMAScript 3rd Edition (ECMA-262)' specification") | Standard | Initial definition. |
| [ECMAScript 5.1 (ECMA-262)  <br>The definition of 'encodeURIComponent' in that specification.](https://www.ecma-international.org/ecma-262/5.1/#sec-15.1.3.4) | Standard |     |
| [ECMAScript 2015 (6th Edition, ECMA-262)  <br>The definition of 'encodeURIComponent' in that specification.](https://www.ecma-international.org/ecma-262/6.0/#sec-encodeuricomponent-uricomponent) | Standard |     |
| [ECMAScript Latest Draft (ECMA-262)  <br>The definition of 'encodeURIComponent' in that specification.](https://tc39.es/ecma262/#sec-encodeuricomponent-uricomponent) | Draft |     |

## Browser compatibility

The compatibility table on this page is generated from structured data. If you'd like to contribute to the data, please check out [https://github.com/mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) and send us a pull request.