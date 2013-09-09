[![Build Status](https://travis-ci.org/bulkan/robotremoteserver.js.png?branch=master)](https://travis-ci.org/bulkan/robotremoteserver.js)

robotremoteserver.js
====================

[![NPM](https://nodei.co/npm/rfremoteserver.png)](https://nodei.co/npm/rfremoteserver.png/) 

[Robot Framework](http://robotframework.googlecode.com/hg/) Remote Server written in Node.js, loosely based on https://github.com/mkorpela/RoboZombie and https://github.com/comick/node-robotremoteserver

Installation
============

`npm install rfremoteserver`


Usage
=====

Create a Object like the following and pass it to an instance of RemoteServer. 

```javascript
var RemoteServer = require('rfremoteserver');

var TestLibrary = {
  my_example_keyword = {
    docs: "an example keyword",
    args: ["*args"],
    function(params, callback) {
      var ret = {};
      // do testy stuff here then either pass the keyword using this.pass or this.fail
      return RemoteServer.pass(callback);
    }
  }
};


var server = new RemoteServer({host: "localhost", port: 4242}, [TestLibrary]);
server.start_remote_server();
```

In the above example, the library is just a JavaScript object with each property being another JavaScript object. These properties are the keywords. 
They are also Objects and need to have three properties. 

* `docs`: String contain the keyword documentation
* `args`: Array of the arguments that keyword accepts. use ["\*args"] if your keyword accepts more than one argument.
* `impl`: The actual keyword function. This function is what gets bound as an xmlrpc method callback hence its args are always `params` & `callback`

Example test case file for Robot Framework to use the above remote keyword library

```
*** Variables ***
${PORT}    8270


*** Settings ***
Library    Remote    http://localhost:${PORT}

*** Test Cases ***

Test Keyword
    ${ret}=   My Example Keyword
    Log    ${ret}
```
