---
title: "How to run Mocha programmatically with custom reporter with code examples"
description: "Code examples to run the mocha tests from your program itself and setup a custom reporter in mocha"
---

We normally run mocha test from the command line, right? But what if we want to run the test from the program itself and write the results of this tests to some file(html, json, txt)

Below I have provided a very generic code that requires a path to test directory as input. It can customized to write the output to file depending on the use case

1. Create a `runner.js` file 

```js
const Mocha = require("mocha"),
    fs = require("fs"),
    path = require("path");

// Instantiate a Mocha instance.
let mocha = new Mocha();
mocha.reporter('./reporter') // path to custom reporter

module.exports = (testDir) =>  {
    // read all files in the `test` directory ending with `test.js` extension
    fs.readdirSync(testDir).filter(filename => filename.substr(-7) === "test.js").forEach(filename => {
        // add the file to run the tests
        mocha.addFile(path.join(testDir, filename));
    });
    
    mocha.run();    
}}
```

2. Create a `reporter.js` file

```js
"use strict";

const Mocha = require("mocha");
const fs = require("fs");
const {
  EVENT_RUN_BEGIN,
  EVENT_RUN_END,
  EVENT_TEST_FAIL,
  EVENT_TEST_PASS,
} = Mocha.Runner.constants; // other constants https://mochajs.org/api/runner.js.html

class Reporter {
    constructor(runner) {
      const stats = runner.stats;
  
      runner
        .once(EVENT_RUN_BEGIN, () => {
          console.log('start');
        })
        .on(EVENT_TEST_PASS, test => {
          console.log(test); // an object containing the test details with `state: passed`
        })
        .on(EVENT_TEST_FAIL, (test, err) => {
          console.log(test); // an object containing the test details with `state: failed`
          console.log(err); // err object containing the debug string for the failed test
        })
        .once(EVENT_RUN_END, () => {
            console.log('Overall stats',stats)
        });
    }
  }
  
  module.exports = Reporter;
```

In the reporter you can use the `fs.appendFileSync()` method of the `fs` module to write the information to the file.
Since we are using a custom reporter the the test reults wont be printed on the command line
### References

[Mocha API documentation](https://mochajs.org/api/mocha)

[Using Mocha programmatically](https://github.com/mochajs/mocha/wiki/Using-Mocha-programmatically)

[Third party reporters](https://github.com/mochajs/mocha/wiki/Third-party-reporters)
