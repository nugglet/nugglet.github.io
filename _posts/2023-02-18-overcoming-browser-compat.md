---
title: 'Overcoming Browser Cross Compatibility Issues'
date: 2023-02-18
permalink: /posts/2023/02/overcoming_browser_compat
tags:
  - SWE
  - testing
  - webdev
excerpt_separator: "<!--more-->"

toc: true
toc_sticky: true
---

From Google Chrome to Firefox to Opera GX, each browser has varying support and implementation for web pages. In this article, I explore the various challenges of developing cross-compatible webpages and some common bugfixes.<!--more-->

## Challenges:

- Browsers have different levels of [CSS support](https://www.w3schools.com/cssref/css3_browsersupport.php) (i.e. fonts, shadows etc.)/ HTML support etc.
- Different ways of rendering HTML
- Mobile vs Desktop can be vastly different
- Color can be displayed differently based on different monitor hardware
- Pixel measurements may vary. The size of the page and its contents may scale badly if they are constants, and have to be adapted to different monitor sizes and resolutions.


## Solutions


### DOCTYPE Error

Remember to include the doctype at the start of your code to ensure it is rendered correctly by browsers. i.e. !DOCTYPE html

Browsers have 2 modes for checking code. 
1. Strict mode: stricter error checking against W3C specifications
2. Quirks mode: less strict checking, usually for providing backwards compat with older browsers
When the DOCTYPE tag is missing, the browser tends to go into quirks mode. If the browser does not support HTML5, it will not understand which version to look for, leading to html tags becoming unresponsive/not interpreted correctly. The webpage will not look correct in this case.


### Browser Detection

One common cross-browser compat issue is when javascript fails to detect the browser (often due to too old browser versions).
To solve this, browser detection can be replaced by a modernizer, which is a collection of tests to check all the features of the browser. Using this, devs can direct the site to focus on features rather than browsers, facilitating a better experience.


### HTML/CSS Validation

Different browsers read and handle code differently. e.g. certain browsers auto lint and correct errors, while others may not.
This can be solved by using validating tools (such as the W3C HTML validator or Jigsaw CSS Validator). 


### CSS Resets

CSS reset style sheets are often defined to avoid layout issues in rendering. This is due to browsers having different default CSS styles. Defining a CSS reset style ensures that the basic 'default' styling is consistent across all browsers.
Some common reset style sheets used include HTML5Reset, [Eric Meyers CSS Reset](https://meyerweb.com/eric/tools/css/reset/) and the Github based [Normalize.css](https://github.com/necolas/normalize.css/blob/master/normalize.css).


### Vendor Specific Functions

This refers to adding a certain prefix to functions, if the function is specific to a certain browser. To ensure proper functionality, the function without the prefix must also be implemented.

Common vendor prefixes include:
- Mozilla Firefox (-moz)
- Internet Explorer (-ms)
- Opera (-o)
- Safari and Chrome (-webkit)


### Website Feature Functionality

This is a preventative measure involving cross-checking feature support with various browsers and using [polyfills](https://philipwalton.github.io/polyfill/) and feature detection to accommodate browser updates and changes.

Polyfill.js is a library that facilitates polyfill writing. A polyfill is code that implements a feature on web browsers that do not natively support the feature. Most often, it refers to a JavaScript library that implements an HTML5 or CSS web standard, either an established standard on older browsers, or a proposed standard on existing browsers.


### Use Cross-browser friendly libs and frameworks

i.e. Angular.js, React.js, bootstrap (CSS), Animate (CSS), JQuery (Scripting)

These tools help developers to bring in structure, scalability, and security to the web applications. Using the wrong alternatives to these libraries can lead to a variety of cross-browser issues that can range from the incorrect working of library features to the complete framework crash.


### Use separate stylesheets for different browsers

It is beneficial to keep the styles separate for each type of browser that the website supports. Once the separation is complete, it can be included within the same HTML page by using conditional comments which help in invoking the right stylesheet for the right type of browser. This helps to prevent stylesheets from turning into styleshit.


### Perform cross-browser testing

This is the final step in ensuring your page is compatible across browsers (diff browser-os combi, devices, or assistive tools). There are a number of tools to do this, but the most common method is through scripting. Cross-browser testing checks for base functionality, design, accessibility and responsiveness. It is usually executed alongside development or during staging/pre-release.

### Progressive Enhancement

Release early, release often.

PE is a bottom up approach (as compared to Graceful Degradation, which is top down). This means that features are built with increasing complexity (starting from the simplest). PE is easier to maintain and provides more feature security for users since the project is starting with a solid foundation.

Layers of PE
1. Clean, semantic HTML
2. CSS
3. Javascript


### Links and Resources

- https://www.browserstack.com/cross-browser-testing
- https://www.smashingmagazine.com/2009/04/progressive-enhancement-what-it-is-and-how-to-use-it/
- https://www.lambdatest.com/blog/10-ways-to-avoid-cross-browser-compatibility-issues/
