# AppAvailability for iOS and Android

[![GitHub version](https://badge.fury.io/gh/ohh2ahh%2FAppAvailability.svg)](https://badge.fury.io/gh/ohh2ahh%2FAppAvailability) [![npm version](https://badge.fury.io/js/cordova-plugin-appavailability.svg)](https://badge.fury.io/js/cordova-plugin-appavailability)

A Plugin for Apache Cordova and Adobe PhoneGap by [ohh2ahh](http://ohh2ahh.com).

1. [Description](https://github.com/ohh2ahh/AppAvailability#1-description)
2. [Installation](https://github.com/ohh2ahh/AppAvailability#2-installation)
	2. [Automatically (Command-line Interface)](https://github.com/ohh2ahh/AppAvailability#automatically-command-line-interface)
	2. [PhoneGap Build](https://github.com/ohh2ahh/AppAvailability#phonegap-build)
3. [Usage](https://github.com/ohh2ahh/AppAvailability#3-usage)
	3. [iOS](https://github.com/ohh2ahh/AppAvailability#ios)
	3. [Android](https://github.com/ohh2ahh/AppAvailability#android)
	3. [Full Example](https://github.com/ohh2ahh/AppAvailability#full-example)
	3. [Old Approach (AppAvailability < 0.3.0)](https://github.com/ohh2ahh/AppAvailability#old-approach-appavailability--030)
4. [Some URI Schemes / Package Names](https://github.com/ohh2ahh/AppAvailability#4-some-uri-schemes--package-names)
5. [License](https://github.com/ohh2ahh/AppAvailability#5-license)

## Important: iOS 9 and iOS 10 URL Scheme Whitelist
Apple changed the `canOpenURL` method on iOS 9. Apps which are checking for URL Schemes have to declare these Schemes as it is submitted to Apple. The article [Quick Take on iOS 9 URL Scheme Changes](http://awkwardhare.com/post/121196006730/quick-take-on-ios-9-url-scheme-changes) expains the changes in detail.

### Add URL Schemes to the Whitelist
Simply open your app's .plist (usually `platforms/ios/<appname>/<appname>-Info.plist)` with an editor and add the following code with your needed Schemes.

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>twitter</string>
    <string>whatsapp</string>
</array>
```

## 1. Description

This plugin allows you to check if an app is installed on the user's device.
It requires an URI Scheme (e.g. twitter://) on iOS or a Package Name (e.g com.twitter.android) on Android.

* Ready for the Command-line Interface of Cordova / PhoneGap 3.0 and later
* Works with PhoneGap Build

### Supported Platforms

* iOS
* Android

## 2. Installation

The Cordova CLI is the recommended way to install AppAvailability, see [The Command-line Interface](http://cordova.apache.org/docs/en/4.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface). You can find the plugin on these registries:
* [GitHub](https://github.com/ohh2ahh/AppAvailability)
* [npm](https://www.npmjs.com/package/cordova-plugin-appavailability)

### Automatically (Command-line Interface)

Simply run this command to add the AppAvailability to your project:
```
$ cordova plugin add https://github.com/jianhuili/AppAvailability.git --save
```

Don't forget to prepare and compile your project:
```
$ cordova prepare
$ cordova build
```

You don't have to reference the JavaScript in your `index.html`.

## 3. Usage

:exclamation: The code changed in version 0.3.0 and supports now success and error callbacks! But you can still use the old approach, which is [described below](https://github.com/ohh2ahh/AppAvailability#old-approach-appavailability--030).

### iOS

```javascript
appAvailability.check(
    'twitter://', // URI Scheme
    function() {  // Success callback
        console.log('Twitter is available');
    },
    function() {  // Error callback
        console.log('Twitter is not available');
    }
);
```

### Android

```javascript
appAvailability.check(
    'com.twitter.android', // Package Name
    function(info) {           // Success callback        
        // Info parameter is available only for android
        console.log('Twitter is available, and it\'s version is ', info.version);
    },
    function() {           // Error callback
        console.log('Twitter is not available');
    }
);
```

### Full Example

```javascript
var scheme;

// Don't forget to add the cordova-plugin-device plugin for `device.platform`
if(device.platform === 'iOS') {
    scheme = 'twitter://';
}
else if(device.platform === 'Android') {
    scheme = 'com.twitter.android';
}

appAvailability.check(
    scheme,       // URI Scheme or Package Name
    function() {  // Success callback
        console.log(scheme + ' is available :)');
    },
    function() {  // Error callback
        console.log(scheme + ' is not available :(');
    }
);
```

### Old Approach (AppAvailability < 0.3.0)

The only thing you have to do is replacing `appAvailability.check` with `appAvailability.checkBool`:

```javascript
appAvailability.checkBool('twitter://', function(availability) {
    // availability is either true or false
    if(availability) { console.log('Twitter is available'); }
});
```

## 4. Some URI Schemes / Package Names

[How do I get the URI Scheme / Package Name?](https://github.com/ohh2ahh/AppAvailability/issues/2#issuecomment-22203591)

Twitter:
* iOS: `twitter://`
* Android: `com.twitter.android`

Facebook:
* iOS: `fb://`
* Android: `com.facebook.katana`

WhatsApp:
* iOS: `whatsapp://` (only since v. 2.10.1, [more information](http://www.whatsapp.com/faq/en/iphone/23559013))
* Android: `com.whatsapp`

## 5. License

[The MIT License (MIT)](http://www.opensource.org/licenses/mit-license.html)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
