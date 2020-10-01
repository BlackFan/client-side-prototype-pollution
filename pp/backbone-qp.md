## backbone-query-parameters

URL: https://github.com/jhudson8/backbone-query-parameters

### Vulnerable code fragment

https://github.com/V4Fire/Core/blob/f28cb1d6209a29cf13c6b5167784b9c81160ed66/src/core/url/convert.ts

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