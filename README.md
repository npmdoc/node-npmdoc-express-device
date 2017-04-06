# api documentation for  [express-device (v0.4.2)](https://github.com/rguerreiro/express-device)  [![npm package](https://img.shields.io/npm/v/npmdoc-express-device.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-express-device) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-express-device.svg)](https://travis-ci.org/npmdoc/node-npmdoc-express-device)
#### Browser detection library, built on top of express

[![NPM](https://nodei.co/npm/express-device.png?downloads=true)](https://www.npmjs.com/package/express-device)

[![apidoc](https://npmdoc.github.io/node-npmdoc-express-device/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-express-device_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-express-device/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-express-device/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-express-device/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Rodrigo Guerreiro",
        "url": "@rguerreiro"
    },
    "bugs": {
        "url": "https://github.com/rguerreiro/express-device/issues"
    },
    "dependencies": {
        "device": ">=0.3.1",
        "express-partials": "0.3.0"
    },
    "description": "Browser detection library, built on top of express",
    "devDependencies": {
        "body-parser": "*",
        "cookie-parser": "*",
        "ejs": "*",
        "express": ">=4.x.x",
        "method-override": "*",
        "mocha": "*",
        "supertest": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "597c822fd4204d46b22faf2ad69ec85936bcb6a3",
        "tarball": "https://registry.npmjs.org/express-device/-/express-device-0.4.2.tgz"
    },
    "engines": {
        "node": ">=0.10"
    },
    "gitHead": "b278e344a1bd2ec23211e897c5fd1439d7ed29e8",
    "homepage": "https://github.com/rguerreiro/express-device",
    "keywords": [
        "browser",
        "mobile",
        "detection",
        "user-agent",
        "useragent",
        "express",
        "layout",
        "view routing",
        "desktop",
        "phone",
        "tablet",
        "tv",
        "bot",
        "car",
        "responsive design",
        "parsing"
    ],
    "license": "MIT",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "rguerreiro",
            "email": "6speedpt@gmail.com"
        }
    ],
    "name": "express-device",
    "optionalDependencies": {},
    "private": false,
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/rguerreiro/express-device.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "0.4.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module express-device](#apidoc.module.express-device)
1.  [function <span class="apidocSignatureSpan">express-device.</span>Parser (user_agent, options)](#apidoc.element.express-device.Parser)
1.  [function <span class="apidocSignatureSpan">express-device.</span>capture (options)](#apidoc.element.express-device.capture)
1.  [function <span class="apidocSignatureSpan">express-device.</span>customCheck (req, mydevice)](#apidoc.element.express-device.customCheck)
1.  [function <span class="apidocSignatureSpan">express-device.</span>enableDeviceHelpers (app)](#apidoc.element.express-device.enableDeviceHelpers)
1.  [function <span class="apidocSignatureSpan">express-device.</span>enableViewRouting (app, options)](#apidoc.element.express-device.enableViewRouting)
1.  string <span class="apidocSignatureSpan">express-device.</span>namespace
1.  string <span class="apidocSignatureSpan">express-device.</span>version



# <a name="apidoc.module.express-device"></a>[module express-device](#apidoc.module.express-device)

#### <a name="apidoc.element.express-device.Parser"></a>[function <span class="apidocSignatureSpan">express-device.</span>Parser (user_agent, options)](#apidoc.element.express-device.Parser)
- description and source-code
```javascript
function DeviceParser(user_agent, options) {
    var self = this;

    self.options = merge(defaultOptions, options);

    self.make_sure_parser_was_executed = function () {
        if (self.options.parseUserAgent === true && !self.useragent)
            self.useragent = useragent.lookup(user_agent);
    };

    self.get_model = function () {
        self.make_sure_parser_was_executed();
        if (self.useragent)
            return self.useragent.device.family;
        return '';
    };

    self.get_type = function () {
        var ua = user_agent;

        if (!ua || ua === '') {
            // No user agent.
            return self.options.emptyUserAgentDeviceType;
        }
        // overwrite Flipboard user agent otherwise it's detected as a desktop
        if (ua.match(/FlipboardProxy/i))
            ua = 'FlipboardProxy/1.1;  http://flipboard.com/browserproxy';
        if (ua.match(/Applebot/i))
            ua = 'Applebot/0.1;  http://www.apple.com/go/applebot';
        if (ua.match(/GoogleTV|SmartTV|SMART-TV|Internet TV|NetCast|NETTV|AppleTV|boxee|Kylo|Roku|DLNADOC|hbbtv|CrKey|CE\-HTML/i
)) {
            // if user agent is a smart TV - http://goo.gl/FocDk
            return 'tv';
        } else if (ua.match(/Xbox|PLAYSTATION (3|4)|Wii/i)) {
            // if user agent is a TV Based Gaming Console
            return 'tv';
        } else if (ua.match(/QtCarBrowser/i)) {
            // if the user agent is a car
            return self.options.carUserAgentDeviceType;;
        } else if (ua.match(/Googlebot/i)) {
            // if user agent is a BOT/Crawler/Spider
            return self.options.botUserAgentDeviceType;
        } else if (ua.match(/iP(a|ro)d/i) || (ua.match(/tablet/i) && !ua.match(/RX-34/i)) || ua.match(/FOLIO/i)) {
            // if user agent is a Tablet
            return 'tablet';
        } else if (ua.match(/Linux/i) && ua.match(/Android/i) && !ua.match(/Fennec|mobi|HTC Magic|HTCX06HT|Nexus One|SC-02B|fone
 945/i)) {
            // if user agent is an Android Tablet
            return 'tablet';
        } else if (ua.match(/Kindle/i) || (ua.match(/Mac OS/i) && ua.match(/Silk/i)) || (ua.match(/AppleWebKit/i) && ua.match(/Silk
/i) && !ua.match(/Playstation Vita/i))) {
            // if user agent is a Kindle or Kindle Fire
            return 'tablet';
        } else if (ua.match(/GT-P10|SC-01C|SHW-M180S|SGH-T849|SCH-I800|SHW-M180L|SPH-P100|SGH-I987|zt180|HTC( Flyer|_Flyer)|Sprint
 ATP51|ViewPad7|pandigital(sprnova|nova)|Ideos S7|Dell Streak 7|Advent Vega|A101IT|A70BHT|MID7015|Next2|nook/i) || (ua.match(/MB511
/i) && ua.match(/RUTEM/i))) {
            // if user agent is a pre Android 3.0 Tablet
            return 'tablet';
        } else if (ua.match(/BOLT|Fennec|Iris|Maemo|Minimo|Mobi|mowser|NetFront|Novarra|Prism|RX-34|Skyfire|Tear|XV6875|XV6975|Google
 Wireless Transcoder/i)) {
            // if user agent is unique phone User Agent
            return 'phone';
        } else if (ua.match(/Opera/i) && ua.match(/Windows NT 5/i) && ua.match(/HTC|Xda|Mini|Vario|SAMSUNG\-GT\-i8000|SAMSUNG\-SGH\-i9/i)) {
            // if user agent is an odd Opera User Agent - http://goo.gl/nK90K
            return 'phone';
        } else if ((ua.match(/Windows (NT|XP|ME|9)/) && !ua.match(/Phone/i)) && !ua.match(/Bot|Spider|ia_archiver|NewsGator/i) ||
ua.match(/Win( ?9|NT)/i)) {
            // if user agent is Windows Desktop
            return 'desktop';
        } else if (ua.match(/Macintosh|PowerPC/i) && !ua.match(/Silk/i)) {
            // if agent is Mac Desktop
            return 'desktop';
        } else if (ua.match(/Linux/i) && ua.match(/X11/i) && !ua.match(/Charlotte|JobBot/i)) {
            // if user agent is a Linux Desktop
            return 'desktop';
        } else if (ua.match(/CrOS/)) {
            // if user agent is a Chrome Book
            return 'desktop';
        } else if (ua.match(/Solaris|SunOS|BSD/i)) {
            // if user agent is a Solaris, SunOS, BSD Desktop
            return 'desktop';
        } else if (ua.m ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.express-device.capture"></a>[function <span class="apidocSignatureSpan">express-device.</span>capture (options)](#apidoc.element.express-device.capture)
- description and source-code
```javascript
function capture(options) {
    return function (req, res, next) {
        var useragent = req.headers['user-agent'];
        var mydevice = device(useragent, options);

        req.device = req.device || {};
        req.device.parser = mydevice.parser; // to expose the device parser object to the running app
        req.device.type = customCheck(req, mydevice);
        req.device.name = mydevice.model;

        if (next) return next();
    };
}
```
- example usage
```shell
...
var device = require('express-device');

app.set('view engine', 'ejs');
app.set('view options', { layout: false });
app.set('views', __dirname + '/views');

app.use(bodyParser());
app.use(device.capture());
'''
By doing this you're enabling the **request** object to have a property called **device**, which have the following properties:
<table>
<tr><td><strong>Name</strong></td><td><strong>Field Type</strong></td><td><strong>Description</strong></td><td><strong>Possible
Values</strong></td></tr>
<tr>
    <td>type</td>
    <td>string</td>
...
```

#### <a name="apidoc.element.express-device.customCheck"></a>[function <span class="apidocSignatureSpan">express-device.</span>customCheck (req, mydevice)](#apidoc.element.express-device.customCheck)
- description and source-code
```javascript
function customCheck(req, mydevice) {
    var useragent = req.headers['user-agent'];

    if (!useragent || useragent === '') {
        if (req.headers['cloudfront-is-mobile-viewer'] === 'true') return 'phone';
        if (req.headers['cloudfront-is-tablet-viewer'] === 'true') return 'tablet';
        if (req.headers['cloudfront-is-desktop-viewer'] === 'true') return 'desktop';
        // No user agent.
        return mydevice.parser.options.emptyUserAgentDeviceType;
    }

    return mydevice.type;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.express-device.enableDeviceHelpers"></a>[function <span class="apidocSignatureSpan">express-device.</span>enableDeviceHelpers (app)](#apidoc.element.express-device.enableDeviceHelpers)
- description and source-code
```javascript
function enableDeviceHelpers(app) {
    var check_request = function (req, res, next) {
        if (typeof req.device === 'undefined') {
            next(new Error('Must enable the device capture by using app.use(device.capture())'));
        } else {
            next();
        }
    };
    app.use(check_request);

    var check_device = function (device) {
        return function (req, res, next) {
            res.locals['is_' + device] = req.device.type === device;
            if (next) {
                next();
            }
        }
    }

    app.use(check_device('desktop'));
    app.use(check_device('phone'));
    app.use(check_device('tablet'));
    app.use(check_device('tv'));
    app.use(check_device('bot'));
    app.use(check_device('car'));

    var is_mobile = function (req, res, next) {
        res.locals.is_mobile = res.locals.is_phone || res.locals.is_tablet;
        if (next) {
            next();
        }
    };
    app.use(is_mobile);

    var device_type = function (req, res, next) {
        res.locals.device_type = req.device.type;
        if (next) {
            next();
        }
    };
    app.use(device_type);
    var device_name = function (req, res, next) {
        res.locals.device_name = req.device.name;
        if (next) {
            next();
        }
    };
    app.use(device_name);
}
```
- example usage
```shell
...
        <td>It returns the device type string parsed from the request</td>
    </tr>
    <tr>
        <td>device_name</td>
        <td>It returns the device name string parsed from the request</td>
    </tr>
</table>
In order to enable this method you have to pass the app reference to **device.enableDeviceHelpers(app)**, just before **app.use(
app.router)**.

Here's an example on how to use them (using [EJS](https://github.com/visionmedia/ejs) view engine):
'''html
<html>
<head>
    <title><%= title %></title>
</head>
...
```

#### <a name="apidoc.element.express-device.enableViewRouting"></a>[function <span class="apidocSignatureSpan">express-device.</span>enableViewRouting (app, options)](#apidoc.element.express-device.enableViewRouting)
- description and source-code
```javascript
function enableViewRouting(app, options) {
    if (!options || options.noPartials === false)
        app.use(partials());

    app.use(function (req, res, next) {
        var _render = res.render.bind(res);
        res.render = function (name, options, fn) {
            var layout = options && options.layout;
            var ignore = (options && options.ignoreViewRouting) || false;
            var deviceType = req.device.type;

            if (options && options.forceType) {
                deviceType = options.forceType;
            }

            if (ignore === false) {
                if (layout === true || layout === undefined) {
                    var defaultLayout = path.join(deviceType, 'layout');
                    options = options || {};
                    if (check(req, res, defaultLayout)) options.layout = defaultLayout;
                }
                else if (typeof layout === "string") {
                    var deviceLayout = path.join(deviceType, layout);
                    if (check(req, res, deviceLayout)) options.layout = deviceLayout;
                }

                var deviceView = path.join(deviceType, name);
                if (check(req, res, deviceView)) name = deviceView;
            }

            _render(name, options, fn);
        };

        if (next) return next();
    });
}
```
- example usage
```shell
...
<% } %>
</body>
</html>
'''

You can check a full working example [here](https://github.com/rguerreiro/express-device/tree/master/example).

In version 0.3.0 a cool feature was added: the ability to route to a specific view\layout based on the device type (you must pass
 the app reference to **device.enableViewRouting(app)** to set it up). Consider the code below:

.
|-- views
    |-- phone
    |    |-- layout.ejs
    |    '-- index.ejs
    |-- layout.ejs
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
