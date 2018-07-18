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

* The [ECMAScript spec](https://www.ecma-international.org/publications/standards/Ecma-262.htm) defines the core of the language: language features, syntax, execution and the standard library.

* ECMAScript does not define a host environment in which a program is executed, it allows for several

* An HTML document in the browser is one possible host environment. Node.js is another popular one.

* The [HTML spec](https://www.w3.org/TR/html5/) defines HTML as a markup language and also defines how JS is executed in the context of an HTML document.

* The [DOM spec](https://dom.spec.whatwg.org/) (and HTML spec) define how JS can access and alter the document.

* The HTML and DOM specs define the main objects that client-side JavaScript is dealing with: nodes, elements and events. Fundamental objects include `window` and `document`.

* There are a lot of other specifications that add more APIs to the browser’s JS environment. These are usually just called Web APIs.

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

## Postel's law

From [RFC 790](https://tools.ietf.org/html/rfc760) (IP):

> In general, an implementation should be conservative in its sending behavior, and liberal in its receiving behavior.

and from [RFC 761](https://tools.ietf.org/html/rfc761) (TCP):

> TCP implementations should follow a general principle of robustness: be conservative in what you do, be liberal in what you accept from others.

The original idea is specific to TCP/IP but can be generalized to all programs that read, parse and process user input, file formats or other structured data.

Actually maybe instead of accepting anything a program can interpret how about a program:

* be explicit about what it accepts
* is outspoken about technical errors
* has a well-defined error handling

## How JS might fail

TODO

## How to prevent failure

TODO