ShioriJK - SHIORI/3.x Parser/Container
=============================================

[![npm](https://img.shields.io/npm/v/shiorijk.svg)](https://www.npmjs.com/package/shiorijk)
[![npm license](https://img.shields.io/npm/l/shiorijk.svg)](https://www.npmjs.com/package/shiorijk)
[![npm download total](https://img.shields.io/npm/dt/shiorijk.svg)](https://www.npmjs.com/package/shiorijk)
[![npm download by month](https://img.shields.io/npm/dm/shiorijk.svg)](https://www.npmjs.com/package/shiorijk)
[![Bower](https://img.shields.io/bower/v/shiorijk.svg)](https://github.com/Narazaka/shiorijk)
[![Bower](https://img.shields.io/bower/l/shiorijk.svg)](https://github.com/Narazaka/shiorijk)

[![Dependency Status](https://david-dm.org/Narazaka/shiorijk.svg)](https://david-dm.org/Narazaka/shiorijk)
[![devDependency Status](https://david-dm.org/Narazaka/shiorijk/dev-status.svg)](https://david-dm.org/Narazaka/shiorijk#info=devDependencies)
[![Travis Build Status](https://travis-ci.org/Narazaka/shiorijk.svg)](https://travis-ci.org/Narazaka/shiorijk)
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/Narazaka/shiorijk?svg=true)](https://ci.appveyor.com/project/Narazaka/shiorijk)
[![codecov.io](https://codecov.io/github/Narazaka/shiorijk/coverage.svg?branch=master)](https://codecov.io/github/Narazaka/shiorijk?branch=master)
[![Code Climate](https://codeclimate.com/github/Narazaka/shiorijk/badges/gpa.svg)](https://codeclimate.com/github/Narazaka/shiorijk)

**Do you know that SHIORI is not JS but JK ?**

Installation
--------------------------

    npm install shiorijk

or

    bower install shiorijk

What is ShioriJK ?
--------------------------

ShioriJK is a library of SHIORI protocol parsers and containers implemented by Javascript (CoffeeScript with TypeScript definition) for making SHIORI subsystem.

Usage overview
--------------------------

[example.ts](example.ts)
```typescript
import * as ShioriJK from "shiorijk";
// const ShioriJK = require("shiorijk"); // also OK

// ---------- parse request ----------

// parse transaction
const requestParser = new ShioriJK.Shiori.Request.Parser();
const requestStr =
  "GET SHIORI/3.0\r\n" +
  "Charset: UTF-8\r\n" +
  "Sender: embryo\r\n" +
  "ID: OnBIFFComplete\r\n" +
  "Reference0: 2\r\n" +
  "Reference1: 1024\r\n" +
  "Reference2: mail.example.com\r\n" +
  "Reference3: 1\r\n" +
  "Reference4: Subject: foo\x01X-Mailer: sendmail\x02Subject: bar\x01X-Mailer: Sylpheed\r\n" +
  "Reference5: 512\x01512\r\n" +
  "Reference6: 123@example.com\x01456@example.com\r\n" +
  "\r\n";
const request = requestParser.parse(requestStr);
console.log(request.request_line.method === "GET"); // request line info
console.log(request.headers.get("ID") === "OnBIFFComplete"); // gets header value
console.log(request.headers.ID === "OnBIFFComplete"); // shortcut for common headers
console.log(request.headers.Reference(1) === "1024"); // shortcut for Reference*
console.log(request.headers.references().length === 7); // get all references
console.log((request.headers.get_separated("Reference6") || [])[0] === "123@example.com"); // separated by \x01
console.log((request.headers.get_separated2("Reference4") || [])[1][0] === "Subject: bar"); // separated by \x01 and \x02

// parse chunks like HTTP
let parseResult;
parseResult = requestParser.parse_chunk("GET SHIORI/3.0\r\nCharset: UTF-8\r\n");
console.log(parseResult.state === "continue");
parseResult = requestParser.parse_chunk("ID: version\r\n\r\n");
console.log(parseResult.state === "end");
console.log(parseResult.results.length === 1);
console.log(parseResult.results[0].headers.ID === "version");

// parse lines
parseResult = requestParser.parse_lines(["GET SHIORI/3.0", "Charset: UTF-8"]);
console.log(parseResult.state === "continue");
parseResult = requestParser.parse_lines(["ID: version", ""]);
console.log(parseResult.state === "end");
console.log(parseResult.results[0].headers.ID === "version");

// ---------- parse response ----------

const responseParser = new ShioriJK.Shiori.Response.Parser();
const responseStr =
  "SHIORI/3.0 200 OK\r\n" +
  "Sender: ikaga\r\n" +
  "Charset: UTF-8\r\n" +
  "Value: 8.2.8\r\n" +
  "\r\n";
const response = responseParser.parse(responseStr);
console.log(response.headers.Value === "8.2.8");

// also you can parse by chunks and lines

// ---------- build request ----------

const request2 = new ShioriJK.Message.Request({
  request_line: {method: "GET", version: "3.0"},
  headers: {
    Charset: "UTF-8",
    Sender: "embryo",
  },
});

// can set after initialize
request2.headers.set("ID", "version");

console.log(request2.toString() === "GET SHIORI/3.0\r\nCharset: UTF-8\r\nSender: embryo\r\nID: version\r\n\r\n");

// ---------- build response ----------

const response2 = new ShioriJK.Message.Response({
  status_line: {code: 200, version: "3.0"},
  headers: {
    Charset: "UTF-8",
    Sender: "ikaga",
    Value: "8.2.8",
  }
});

console.log(response2.toString() === "SHIORI/3.0 200 OK\r\nCharset: UTF-8\r\nSender: ikaga\r\nValue: 8.2.8\r\n\r\n");
```

MORE: See the SHIORI implementation [MiyoJS](https://github.com/Narazaka/miyojs.git) that is using ShioriJK.

API Document
--------------------------

See [http://narazaka.github.io/shiorijk/](http://narazaka.github.io/shiorijk/) or the source in src/.

Also you can found the code snippets in test/.

Build
--------------------------

    git submodule init
    git submodule update
    npm install
    gulp

License
--------------------------

This is released under [MIT License](http://narazaka.net/license/MIT?2017).
