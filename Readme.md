# selectn [![Build Status](https://travis-ci.org/wilmoore/selectn.png?branch=master)](https://travis-ci.org/wilmoore/selectn) [![Build Status](https://david-dm.org/wilmoore/selectn.png)](https://david-dm.org/wilmoore/selectn) [![NPM version](https://badge.fury.io/js/selectn.png)](http://badge.fury.io/js/selectn)

  Resolves deeply-nested object properties via dot or bracket-notation for [Node.js][] and the browser.

#### So you can do:

    selectn('info.name.full', person)

#### instead of:

    person && person.info && person.info.name && person.info.name.full

## Features

  - Resolves deeply-nested object properties including dashed (`-`) keys and arrays.
  - Assists in avoiding `if (obj && obj.a && obj.a.b && obj.a.b.c) { return obj.a.b.c; }`.
  - Assists in avoiding `Cannot read property '...' of undefined` **TypeError**.
  - Supports multiple levels of array nesting (i.e. `group[0].section.a.seat[3]`).
  - Supports dashed key access (i.e. `stats.temperature-today`).
  - `selectn` is a [curried][curry] function; thus, partial application is also supported.
  - Functions generated by `selectn` can be passed to higher-order functions like [map][map] or [filter][filter].
  - ES3, ES5, CommonJS, AMD, and legacy-global compatible.

## Non-Features

  - No [eval][] or [Function][] (see: [`eval`][note] in disguise).
  - No [typeof][] since, [typeof][] is not a real solution to this problem but can _appear_ to be due to the way the global scope is _implied_.

## Installation

[component](http://component.io/wilmoore/selectn)

    $ component install wilmoore/selectn

[bower](http://sindresorhus.com/bower-components/)

    $ bower install selectn

[npm](https://npmjs.org/package/selectn)

[![NPM](https://nodei.co/npm/selectn.png?downloads=true)](https://nodei.co/npm/selectn/)

[jam](http://jamjs.org/packages/#/details/selectn)

    $ jam install selectn

[volo](http://volojs.org)

    $ volo add wilmoore/selectn

[manual][]

1. download

        % curl -#O https://raw.github.com/wilmoore/selectn/master/selectn.js

2. use

        <script src="selectn.js"></script>

## Examples

- [Nested property access](#nested-property-access)
- [Avoid TypeError](#avoid-typeerror)
- [Dashed keys](#dashed-keys)
- [Iterator](#iterator)
- [Predicate](#predicate)
- [Callback](#callback)

### Nested property access

Given the following object:

    var talk = {
      info: { name: 'Go Ahead, Make a Mess' }
    };

Apply the `selectn` function to the `path` and `object` parameters for error-free access to deeply nested properties.

    selectn('info.name', talk);
    // => 'Go Ahead, Make a Mess'

### Avoid TypeError

Avoid the dreaded `Cannot read property '...' of undefined` **TypeError**. Instead, `selectn` will return `undefined`.

    function getName(talk) {
      // NOTE: as called below, `talk` is `undefined`
      return selectn('info.name', talk);
    }

    getName();
    //=> undefined

### Dashed keys

Given the following object:

    var talk = {
      info: { 'attendee-count': 200 }
    };

Apply the `selectn` function to the `path` and `object` parameters for error-free access to deeply nested properties.

    selectn('info.attendee-count', talk);
    // => 200

### Iterator

Given the following list:

    var talks  = [
      { info: { name: 'Go Ahead, Make a Mess' }},
      { info: { name: 'Silex Anatomy' }},
      { info: { name: 'Unit Testing in Python' }},
      { info: { name: 'Setting the Stage' }}
    ];

The generated function can be used as a predicate for [map][]:

    var query = selectn('info.name');
    //=> [Function]

    talks.map(query);
    // => [ 'Go Ahead, Make a Mess', 'Silex Anatomy', 'Unit Testing in Python', 'Setting the Stage' ]

### Predicate

Given the following object of language strings:

    var language = [
      { strings: { en: { name: 'english' } }},
      { strings: { es: { name: 'spanish' } }},
      { strings: { km: { name: 'khmer'   } }},
      { strings: { es: { name: 'spanish' } }},
    ];

The generated function can be used as a predicate for [filter][]:

    var spanish = selectn('strings.es');
    //=> [Function]

    language.filter(spanish).length;
    //=> 2

### Callback

You expect the following JSON data from an XMLHttpRequest:

    var data = { Client: { Message: { id: d50afb80-a6be-11e2-9e96-0800200c9a66 } } };

Access the `Client.Message.id` property and log the result to the console:

    $.ajax({...})
      .then(selectn('Client.Message.id'))
      .then(console.log.bind(console));

    //=> d50afb80-a6be-11e2-9e96-0800200c9a66

While this example assumes a [promises][] API, this is applicable with any API which takes a function and returns the result of applying that function.

## Inspiration

- [to-function][]
- [reach][]
- [dref][]

## License

  MIT

[to-function]: https://github.com/component/to-function
[reach]:       https://github.com/spumko/hoek#reachobj-chain
[curry]:       http://benalman.com/news/2012/09/partial-application-in-javascript/#currying
[dref]:        https://github.com/crcn/dref.js
[Function]:    https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Function
[eval]:        https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/eval
[note]:        https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/Member_Operators#Note_on_eval
[typeof]:      https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/typeof
[promises]:    http://promises-aplus.github.io/promises-spec/
[map]:         https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map
[filter]:      https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
[manual]:      http://yuiblog.com/blog/2006/06/01/global-domination/
[Node.js]:     http://nodejs.org

