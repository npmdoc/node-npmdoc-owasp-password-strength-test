# api documentation for  [owasp-password-strength-test (v1.3.0)](https://github.com/nowsecure/owasp-password-strength-test)  [![npm package](https://img.shields.io/npm/v/npmdoc-owasp-password-strength-test.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-owasp-password-strength-test) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-owasp-password-strength-test.svg)](https://travis-ci.org/npmdoc/node-npmdoc-owasp-password-strength-test)
#### A password-strength tester based upon the OWASP guidelines for enforcing strong passwords.

[![NPM](https://nodei.co/npm/owasp-password-strength-test.png?downloads=true)](https://www.npmjs.com/package/owasp-password-strength-test)

[![apidoc](https://npmdoc.github.io/node-npmdoc-owasp-password-strength-test/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-owasp-password-strength-test_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-owasp-password-strength-test/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-owasp-password-strength-test/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-owasp-password-strength-test/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Chris Allen Lane"
    },
    "bugs": {
        "url": "https://github.com/nowsecure/owasp-password-strength-test/issues"
    },
    "dependencies": {},
    "description": "A password-strength tester based upon the OWASP guidelines for enforcing strong passwords.",
    "devDependencies": {
        "jshint": "2.6.3",
        "mocha": "2.2.4",
        "should": "3.1.2"
    },
    "directories": {},
    "dist": {
        "shasum": "4f629e42903e8f6d279b230d657ab61e58e44b12",
        "tarball": "https://registry.npmjs.org/owasp-password-strength-test/-/owasp-password-strength-test-1.3.0.tgz"
    },
    "gitHead": "a8545f211fd90108fc976c31ff53fbf0d5316be2",
    "homepage": "https://github.com/nowsecure/owasp-password-strength-test",
    "jshintConfig": {
        "expr": true,
        "laxbreak": true
    },
    "keywords": [
        "security",
        "password",
        "owasp"
    ],
    "license": "MIT",
    "main": "owasp-password-strength-test.js",
    "maintainers": [
        {
            "name": "chrisallenlane",
            "email": "chris+npm@chris-allen-lane.com"
        }
    ],
    "name": "owasp-password-strength-test",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/nowsecure/owasp-password-strength-test.git"
    },
    "scripts": {
        "lint": "jshint *.js",
        "test": "mocha --recursive --reporter spec test.js"
    },
    "version": "1.3.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module owasp-password-strength-test](#apidoc.module.owasp-password-strength-test)
1.  [function <span class="apidocSignatureSpan">owasp-password-strength-test.</span>config (params)](#apidoc.element.owasp-password-strength-test.config)
1.  [function <span class="apidocSignatureSpan">owasp-password-strength-test.</span>test (password)](#apidoc.element.owasp-password-strength-test.test)
1.  object <span class="apidocSignatureSpan">owasp-password-strength-test.</span>configs
1.  object <span class="apidocSignatureSpan">owasp-password-strength-test.</span>tests



# <a name="apidoc.module.owasp-password-strength-test"></a>[module owasp-password-strength-test](#apidoc.module.owasp-password-strength-test)

#### <a name="apidoc.element.owasp-password-strength-test.config"></a>[function <span class="apidocSignatureSpan">owasp-password-strength-test.</span>config (params)](#apidoc.element.owasp-password-strength-test.config)
- description and source-code
```javascript
config = function (params) {
  for (var prop in params) {
    if (params.hasOwnProperty(prop) && this.configs.hasOwnProperty(prop)) {
      this.configs[prop] = params[prop];
    }
  }
}
```
- example usage
```shell
...


'''javascript
var owasp = require('owasp-password-strength-test');

// Pass a hash of settings to the 'config' method. The settings shown here are
// the defaults.
owasp.config({
  allowPassphrases       : true,
  maxLength              : 128,
  minLength              : 10,
  minPhraseLength        : 20,
  minOptionalTestsToPass : 4,
});
'''
...
```

#### <a name="apidoc.element.owasp-password-strength-test.test"></a>[function <span class="apidocSignatureSpan">owasp-password-strength-test.</span>test (password)](#apidoc.element.owasp-password-strength-test.test)
- description and source-code
```javascript
test = function (password) {

  // create an object to store the test results
  var result = {
    errors              : [],
    failedTests         : [],
    passedTests         : [],
    requiredTestErrors  : [],
    optionalTestErrors  : [],
    isPassphrase        : false,
    strong              : true,
    optionalTestsPassed : 0,
  };

  // Always submit the password/passphrase to the required tests
  var i = 0;
  this.tests.required.forEach(function(test) {
    var err = test(password);
    if (typeof err === 'string') {
      result.strong = false;
      result.errors.push(err);
      result.requiredTestErrors.push(err);
      result.failedTests.push(i);
    } else {
      result.passedTests.push(i);
    }
    i++;
  });

  // If configured to allow passphrases, and if the password is of a
  // sufficient length to consider it a passphrase, exempt it from the
  // optional tests.
  if (
    this.configs.allowPassphrases === true &&
    password.length >= this.configs.minPhraseLength
  ) {
    result.isPassphrase = true;
  }

  if (!result.isPassphrase) {
    var j = this.tests.required.length;
    this.tests.optional.forEach(function(test) {
      var err = test(password);
      if (typeof err === 'string') {
        result.errors.push(err);
        result.optionalTestErrors.push(err);
        result.failedTests.push(j);
      } else {
        result.optionalTestsPassed++;
        result.passedTests.push(j);
      }
      j++;
    });
  }

  // If the password is not a passphrase, assert that it has passed a
  // sufficient number of the optional tests, per the configuration
  if (
    !result.isPassphrase &&
    result.optionalTestsPassed < this.configs.minOptionalTestsToPass
  ) {
    result.strong = false;
  }

  // return the result
  return result;
}
```
- example usage
```shell
...

### Server-side ###
'''javascript
// require the module
var owasp = require('owasp-password-strength-test');

// invoke test() to test the strength of a password
var result = owasp.test('correct horse battery staple');
'''

### In-browser ###
'''javascript
// in the browser, including the script will make a
// 'window.owaspPasswordStrengthTest' object availble.
var result = owaspPasswordStrengthTest.test('correct horse battery staple');
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
