---
title: "JSON.parse likes numbers 😱"
date: 2024-07-08
description: "Odd behaviour I discovered with JSON.parse."
summary: "I discoverd JSON parse will accept a number a return a valid response."
tags: ["web development", "json"]
---
While working on a proof of concept for Google and Apple wallet passes (Full blog post to come), I discovered an odd behaviour with `JSON.parse` where I was, due to an error in my code, passing a number to it and it was returning a valid response.

My initial expectation was this would throw an error as you wouldn't typically think of a number as a valid JSON object.

So I dug into this and discovered the JSON specification allows for valid JSON strings to contain primitive values like numbers, strings, booleans, and null.

According to the JSON specification [RFC 8259](https://datatracker.ietf.org/doc/html/rfc8259), a JSON value can be:
- An object e.g `{ "key": "value" }`
- An array e.g `["test", "test2", "test3"]`
- A number e.g `123`
- A string of numbers e.g `123"`
- A boolean (true or false) e.g `true`1
- The null value e.g `null`

So in my case, `JSON.parse` was treating the number as a valid JSON string representing a number.
Here are some more examples considered valid by `JSON.parse`:

```javascript
console.log(JSON.parse(123)) // 123
console.log(JSON.parse('123')) // 123
console.log(JSON.parse(true)) // true
console.log(JSON.parse(null)) // null
```

These all work because JSON.parse() first converts them to their string representation before parsing. 

Another quirk is that `console.log(JSON.parse("hello world"))` will throw an error because the string is not a valid JSON string but `console.log(JSON.parse('"hello world"'))` will return `"hello world"`.

Typically you'd expect to pass a JSON string to `JSON.parse` e.g `JSON.parse('{"key": "value"}')` but it's important to remember that it can also handle primitive values like numbers, strings, booleans, and null and you may get an unexpected "false positive".


