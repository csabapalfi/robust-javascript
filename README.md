# Robust client-side JavaScript

Notes for [https://molily.de/robust-javascript](https://molily.de/robust-javascript).

## Characteristics of JS

HMTL and CSS are declarative and obey the [law of least power](https://www.w3.org/2001/tag/doc/leastPower.html).

JS is full fledged programming language.

When HTML and CSS fails the impact is usually limited. They are designed to be parsed in a forgiving and fault tolerant way.

A single JS error can be catastrophic and make the whole page unusable.

## The browser as runtime

The web is an open, vendor-independent, heterogenous publishing platform.

Held together by several technical standards of different quality.

Browser vendors agree and disagree, some things are standardized some are not.

Different browsers (and versions) on different devices and OS with different network and hardware capabilities.

> Do not take anything for granted. Do not count on anything. Question your beliefs.

## JS standards

Not a single spec but a bunch of them:

* The [ECMAScript spec](https://www.ecma-international.org/publications/standards/Ecma-262.htm) defines the core of the language: language features, syntax, execution and the standard library.

* ECMAScript does not define a host environment in which a program is executed, it allows for several

* An HTML document in the browser is one possible host environment. Node.js is another popular one.

* The [HTML spec](https://www.w3.org/TR/html5/) defines HTML as a markup language and also defines how JS is executed in the context of an HTML document.

* The [DOM spec](https://dom.spec.whatwg.org/) (and HTML spec) define how JS can access and alter the document.

* The HTML and DOM specs define the main objects that client-side JavaScript is dealing with: nodes, elements and events. Fundamental objects include `window` and `document`.

* There are a lot of other specifications that add more APIs to the browser’s JS environment. These are usually just called Web APIs.