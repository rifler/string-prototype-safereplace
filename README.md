# String.prototype.safeReplace

Status: This proposal has not been presented to TC39 yet.

## Motivation

The goal of this proposal is to provide the safer and faster analogue to the [old good String.prototype.replace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#Specifying_a_string_as_a_parameter) method.

According to the docs, the first argument can be a regexp/string (**pattern**) and the second argument can be a string/function (**replacement**).

If we use **regexp pattern**, we can use [legacy RegExp $1-$9 placeholders](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/n) in **string replacement**:

```javascript
var str = 'John Smith';
str.replace(/(\w+)\s(\w+)/, '$2, $1'); // "Smith, John"
```

That's ok, but what is happening here?
```javascript
'foo'.replace('foo', '$&-$&-$&'); // "foo-foo-foo"
```

We **don't use regexp pattern**, but result is strange.

The thing is that **string replacement** can contain [special replacement patterns](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#Specifying_a_string_as_a_parameter):
- `$$` - inserts a "$".
- `$&` - inserts the matched substring.
- ``$` `` - inserts the portion of the string that precedes the matched substring.
- `$'` - inserts the portion of the string that follows the matched substring.
