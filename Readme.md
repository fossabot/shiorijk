ShioriJK - SHIORI/3.x Parser/Container
=============================================

**Do you know that SHIORI is not JS but JK ?**

Installation
--------------------------

    npm install shiorijk

    bower install shiorijk

What is ShioriJK ?
--------------------------

ShioriJK is a library of SHIORI protocol parsers and containers implemented by Javascript (CoffeeScript) for making SHIORI subsystem.

### Usage

See the SHIORI implementation [MiyoJS](https://github.com/Narazaka/miyojs.git) that is using ShioriJK.

Overview
--------------------------

Pass SHIORI request transaction or its chunk to ShioriJK then it parses the request and returns parsed request data in the container.

Document
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

This is released under [MIT License](http://narazaka.net/license/MIT?2014).
