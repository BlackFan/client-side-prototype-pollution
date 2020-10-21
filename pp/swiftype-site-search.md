## Swiftype Site Search (uses jQuery BBQ)

URL: https://swiftype.com/documentation/site-search/crawler-quick-start

### JS Fingerprint
```
return (typeof SwiftypeObject != 'undefined')
```

### Vulnerable code fragment

https://s.swiftypecdn.com/install/v2/st.js

```js
n.Utils.convertQueryParamsToObject = function(t) {
  return e.deparam(t)
}

t.deparam = h = function(e, n) {
    var i = Object.create(null)  // FIX
      , r = {
        "true": !0,
        "false": !1,
        "null": null
    };
    return t.each(e.replace(/\+/g, " ").split("&"), function(e, o) {
        var s, a = o.split("="), u = b(a[0]), c = i, h = 0, p = u.split("]["), f = p.length - 1;
        if (/\[/.test(p[0]) && /\]$/.test(p[f]) ? (p[f] = p[f].replace(/\]$/, ""),
        p = p.shift().split("[").concat(p),
        f = p.length - 1) : f = 0,
        2 === a.length)
            if (s = b(a[1]),
            n && (s = s && !isNaN(s) ? +s : "undefined" === s ? l : r[s] !== l ? r[s] : s),
            f)
                for (; h <= f; h++)
                    u = "" === p[h] ? c.length : p[h],
                    c = c[u] = h < f ? c[u] || (p[h + 1] && isNaN(p[h + 1]) ? Object.create(null) : []) : s;
            else
                t.isArray(i[u]) ? i[u].push(s) : i[u] !== l ? i[u] = [i[u], s] : i[u] = s;
        else
            u && (i[u] = n ? l : "")
    }),
    i
}    
```

### PoC
```
#__proto__[test]=test
```