# Robust client-side JavaScript

Notes for [https://molily.de/robust-javascript](https://molily.de/robust-javascript).

## Introduction

### Characteristics of JS

HMTL and CSS are declarative and obey the [law of least power](https://www.w3.org/2001/tag/doc/leastPower.html).

JS is full fledged programming language.

When HTML and CSS fails the impact is usually limited. They are designed to be parsed in a forgiving and fault tolerant way.

A single JS error can be catastrophic and make the whole page unusable.

### The browser as runtime

The web is an open, vendor-independent, heterogenous publishing platform.

Held together by several technical standards of different quality.

Browser vendors agree and disagree, some things are standardized some are not.

Different browsers (and versions) on different devices and OS with different network and hardware capabilities.

> Do not take anything for granted. Do not count on anything. Question your beliefs.

### JS standards

Not a single spec but a bunch of them:

The [ECMAScript spec](https://www.ecma-international.org/publications/standards/Ecma-262.htm) defines the core of the language: language features, syntax, execution and the standard library.

ECMAScript does not define a host environment in which a program is executed, it allows for several.

An HTML document in the browser is one host environment (and Node.js is another one).

The [HTML spec](https://www.w3.org/TR/html5/) defines HTML as a markup language and also defines how JS is executed in the context of an HTML document.

The [DOM spec](https://dom.spec.whatwg.org/) (and HTML spec) define how JS can access and alter the document.

The HTML and DOM specs define the main objects that client-side JavaScript is dealing with: nodes, elements and events. Fundamental objects include `window` and `document`.

There are a lot of other specifications that add more APIs to the browser’s JS environment. These are collectively called [Web APIs](https://developer.mozilla.org/en-US/docs/Web/API).

## Achieving Robustness

> Performs well not only under ordinary conditions but also under unusual conditions that stress its designers’ assumptions.

making informed assumptions

### Graceful degradation

Building a full-featured website and adding fallbacks for clients that lack certain capabilities.

i.e. don't fail when a requirement is not met but fall back to a simpler version.

### Progressive enhancement

Start with a minimal feature set that has low barrier of entry (e.g. built only on well established browser features).

Enhance the inital version if more features can be made available (usually based on feature/version checks).

i.e. enhancing but only if we can, staying robust and accessible to browsers/devices with restricted capabilities.

### Graceful degradation vs progressive enhancement

Both has pros and cons in different contexts and can be used together.

Progressive enhancement is usually considered offering more benefits.

Both rely on checking client capabilities with feature detection.

### Fault tolerance

The whole system continues to operate even if sub-systems fail.

Differentiate critical and non-critical sub-systems.

Fault tolerance in JS: dividing the code into independent, sandboxed sub-systems. Only few of them should be critical.

No definition of native sandboxes in JS (yet).

We can employ existing techniques like try..catch and Promises to achieve the desired effect.

### Postel's law

Jon Postel in [RFC 790](https://tools.ietf.org/html/rfc760) (IP):

> In general, an implementation should be conservative in its sending behavior, and liberal in its receiving behavior.

and in [RFC 761](https://tools.ietf.org/html/rfc761) (TCP):

> TCP implementations should follow a general principle of robustness: be conservative in what you do, be liberal in what you accept from others.

The original idea is specific to TCP/IP but can be generalized to all programs that read, parse and process user input, file formats or other structured data.

Actually maybe instead of accepting anything (that a program can interpret) how about a program:

* be explicit about what it accepts
* is outspoken about technical errors
* has a well-defined error handling

## How JS might fail

### Bots without or limited JS support

Most bots ignore JS or don't execute it as normal browsers.

e.g. Google bots execute JS with a timeout.

Also bots might be based on different/old browser versions.

e.g. GoogleBot based on Chrome 41 or bots based on PhantomJS that uses an old version of WebKit.

Most page content ideally should still be available without JS.

### Users with JS disabled or blocked

Even real users might block JS for good reasons like ads.

Blocking is typically based domain or path patterns. Obviously it's best to avoid anything suspicious (e.g. `ads.js`).

Also sometimes corporate/ISP proxies might inject or block scripts.

### Network and loading errors

In the age of mobile/wireless web access flaky connections are the norm. Network connection can be slow or interrupted altogether.

Other types of content deal with this better (e.g. half the HTML can be rendered, progressive image loading, etc) but scripts are all or nothing.

CDNs can help reduce latency, limiting the risk of connection interruption a bit more.

Apart from connection interruption the HTTP request for a script might also fail with an error code like 404 or 500. This might be trivial but still common.

### Parsing errors

A JS parser reads the source code and turns it into abstract syntaxt tree.

If it encounters a character that is not expected in a certain place, it immediately aborts parsing the current script and throws a `SyntaxError`.

Simplest cause: typos, use a linter to catch that.

Another common cause: new JS language features not unterstood by older browsers. (e.g. async-await). Be sure to properly transpile your code for your supported browsers.

### Conflicting scripts

In ECMAScript, the top-most object is called the global object. In the browser, the global object is called `window`.

`window` contains hundreds of built-in properties, the ECMAScript core objects and most browser APIs.

`window` also forms the outermost scope for names defined by scripts.

The global object is a public space shared by browser APIs and all scripts running on a page.

A script overwriting something on the global object can break another. Avoid global names wherever possible.

### Reference errors

A `ReferenceError` is thrown when the program references a name — an identifier in ECMAScript terminology — that cannot be resolved to a value.

The JS engines walks the [scope chain](http://ryanmorr.com/understanding-scope-and-context-in-javascript/) looking for each name used in your program.

ReferenceErrors happen when the code uses an identifier that cannot be found in the current scope and all parent scopes.

Could be a typo. Use a linter.

Another common error is assuming a browser support a specific Web API so that a global identifier will be availabel (e.g. `fetch`) but it isn't (e.g. in an older browser).

Use feature detection. (e.g. most basic: check for the names you want to use):

```js
if (typeof fetch === 'function') {
  /* Call fetch() */
}
```

But be careful as simply verifying a name is available and has the correct type is not always enough:

    * some browsers might have partial support for an API
    * security and privacy preferences might limit some APIs
    * each API could deal with error handling in a different way

### Type errors

TypeError is thrown when you try to do something with a value that its type doesn't support.

Trying to use the call operator `()` on something that's not a function (e.g. `undefined` or anything else): `TypeError: x is not a function` ( e.g. browser doesn't support API we're trying to call as a function
or script defining user-defined function was not loaded)

Trying to redefine a const: `TypeError: Assignment to constant variable.`

Trying to add a property to an Object that was `Object.seal`ed: `TypeError: Cannot add property x, object is not extensible`

Trying overwrite properties on an Object that `Object.freeze`ed: `TypeError: Cannot assign to read only property 'x' of object`

Lint, test, use and editor with static code analysis.