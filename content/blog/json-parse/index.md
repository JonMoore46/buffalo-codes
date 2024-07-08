---
title: "JSON.parse likes numbers 😱"
date: 2024-06-13
description: "Odd behaviour I discovered with JSON.parse."
summary: "I discoverd JSON parse will accept a number a return a valid response."
tags: ["web development", "json"]
---
While working on a proof of concept for Google and Apple wallet passes (Full blog post to come), I discovered, and I can't remember exactly how, an odd behaviour with `JSON.parse` where passing a number to it will return a valid response.

```javascript
const number = 123;
const parsed = JSON.parse(number);
console.log(parsed); // 123
```

I dug into this and discovered the JSON specification allows for valid JSON strings to contain primitive values like numbers, strings, booleans, and null.

According to the JSON specification [RFC 7159](https://datatracker.ietf.org/doc/html/rfc7159.html), a JSON value can be:
- An object e.g `{ "key": "value" }`
- An array e.g `["test", "test2", "test3"]`
- A number e.g `123`
- A string e.g `"hello world"`
- A boolean (true or false) e.g `true`
- The null value e.g `null`

When you pass a number like 1 to JSON.parse(), it is treated as a valid JSON string representing a number. The parsed result will simply be the number itself, therefore `JSON.parse(1)` will return `1` and not actually throw an error.

I can't remember exactly how this happened, but I thought it was interesting and worth sharing.