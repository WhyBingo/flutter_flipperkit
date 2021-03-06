# flutter_flipperkit

[![pub package](https://img.shields.io/pub/v/flutter_flipperkit.svg)](https://pub.dartlang.org/packages/flutter_flipperkit)

English | [简体中文](./README.zh_CN.md)

## Introduction

*Flipper SDK for Flutter helps you debug Flutter apps running in an emulator/simulator or connected physical development devices.*

### Features

- Network inspector
![network](https://fbflipper.com/docs/assets/network.png)

- Shared preferences (and UserDefaults) inspector
![shared-preferences](https://fbflipper.com/docs/assets/shared-preferences.png)

## Quick Start

### Prerequisites

Before starting make sure you have:

- Installed [Flipper Desktop](https://fbflipper.com/docs/getting-started.html)

### Installation

Add this to your package's pubspec.yaml file:

```yaml
dependencies:
  flutter_flipperkit: ^0.0.2
```

Change your project `ios/Podfile` file according to the example:

```diff
+source 'https://github.com/facebook/flipper.git'
+source 'https://github.com/CocoaPods/Specs'
# Uncomment this line to define a global platform for your project
-# platform :ios, '9.0'
+platform :ios, '9.0'
```

Change your project `android/app/build.gradle` file according to the example:

```diff
android {
-    compileSdkVersion 27
+    compileSdkVersion 28

    defaultConfig {
-        targetSdkVersion 27
+        targetSdkVersion 28
    }
}
```

You can install packages from the command line:

```bash
$ flutter packages get
```

### Usage

```dart
import 'package:flutter_flipperkit/flutter_flipperkit.dart';

void main() {
  FlipperClient flipperClient = FlipperClient.getDefault();

  flipperClient.addPlugin(new FlipperNetworkPlugin());
  flipperClient.addPlugin(new FlipperSharedPreferencesPlugin());
  flipperClient.start();

  Random random = new Random();
  String identifier = '${random.nextInt(9999)}';
  RequestInfo requestInfo = RequestInfo(
    requestId: identifier,
    timeStamp: new DateTime.now().millisecondsSinceEpoch,
    headers: new Map()
      ..putIfAbsent("Content-Type", () => "application/json"),
    method: 'POST',
    uri: 'https://api.example.com/account/login',
    body: new Map()
       ..putIfAbsent("username", () => 'lijy91')
       ..putIfAbsent("password", () => "qDrTBZk8jgbA"),
  );
  ResponseInfo responseInfo = ResponseInfo(
    requestId: identifier,
    timeStamp: new DateTime.now().millisecondsSinceEpoch,
    statusCode: 200,
    headers: new Map()
      ..putIfAbsent("Server", () => "Apache/2.4.1 (Unix)")
      ..putIfAbsent("Content-Type", () => "application/json"),
    body: new Map()
      ..putIfAbsent("code", () => 0)
      ..putIfAbsent("message", () => "login successful"),
  );

  FlipperNetworkPlugin flipperNetworkPlugin = FlipperClient.getDefault().getPlugin(FlipperNetworkPlugin.ID);
  flipperNetworkPlugin.reportRequest(requestInfo);
  flipperNetworkPlugin.reportResponse(responseInfo);

  runApp(MyApp());
}

...

```

### Run the app

```bash
$ flutter run
```

## Discussion

If you have any suggestions or questions about this project, you can discuss it by [Telegram](https://t.me/lijy91) or my wechat.

![](http://blankapp.org/assets/images/wechat_qrcode.png)

## License

```
MIT License

Copyright (c) 2019 JianyingLi <lijy91@foxmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```