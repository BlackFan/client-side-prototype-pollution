## php.js parse_str

URL:  
https://github.com/hirak/phpjs/blob/ead6d1a542a0ce0b3969e2c1ea42e9213d12214d/functions/strings/parse_str.js  
https://github.com/locutusjs/locutus/blob/e0a68222d482d43164e96ab96023b712d25680a6/src/php/strings/parse_str.js

### Vulnerable code fragment

https://github.com/hirak/phpjs/blob/ead6d1a542a0ce0b3969e2c1ea42e9213d12214d/functions/strings/parse_str.js#L91-L100

```js
function parse_str(str, array) {

  ...

  for (i = 0; i < sal; i++) {
    tmp = strArr[i].split('=');
    key = fixStr(tmp[0]);
    value = (tmp.length < 2) ? '' : fixStr(tmp[1]);

    ...
    
    if (key && key.charAt(0) !== '[') {

      ...

      for (j = 0, keysLen = keys.length; j < keysLen; j++) {
        key = keys[j].replace(/^['"]/, '')
          .replace(/['"]$/, '');
        lastIter = j !== keys.length - 1;
        lastObj = obj;
        if ((key !== '' && key !== ' ') || j === 0) {
          if (obj[key] === undef) {
            obj[key] = {};
          }
          obj = obj[key];
```

### PoC
```html
<script src="https://raw.githack.com/hirak/phpjs/master/functions/strings/parse_str.js"></script>
<script>
  parse_str(location.search.slice(1));
</script>
```

```
?__proto__[test]=test
?constructor[prototype][test]=test
```
