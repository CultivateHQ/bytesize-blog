---
layout: post
title: Server-side testing with Jest, without jsdom
date: 2019-01-09 12:31:03 +0000
author: Dan Munckton
categories: javascript testing jest
---

You can use [Jest](https://jestjs.io) to test both server-side components and browser components (which need [JSDOM](https://github.com/jsdom/jsdom)). In both cases, the default environment is `jsdom`, which means it’s used even for your server-side tests. This can cause problems, and apparently makes non-DOM tests run slower.

Switch environment in your server-side tests (e.g. I am testing an Express middleware component) by putting the following comment in the header:

```javascript
/**
 * @jest-environment node
 */
 ```

 See [the Jest documentation](https://jestjs.io/docs/en/configuration.html#testenvironment-string).

I stumbled upon this because I wanted to deliberately create a Network Error for my test. Even though I was catching it correctly, `jsdom`’s `xhrutils.js` was handling the HTTP request and logging the error to the `console` independently, which left a noisy stack trace in the test output for a successful test run.

Changing the environment to `node` silenced the logging and meant the actual error was getting through to my exception handling code.
