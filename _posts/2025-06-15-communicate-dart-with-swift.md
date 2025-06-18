---
layout: post
title: 'Communicate Dart with Swift'
tags: [Flutter, Dart, Swift]
featured_image_thumbnail: assets/images/posts/2025/platform_channels/platform_channel_thumbnail.jpg
featured_image: assets/images/posts/2025/platform_channels/platform_channel.jpg
---

# Stuck Between Dart and Native? Flutter Platform Channels to the Rescue!

<!--more-->

So, you're building your dream Flutter app. You've embraced the power of Dart, the beauty of widgets, and the rapid development cycle that Flutter offers. Everything is humming along nicely until you hit a wall. You need to integrate a fantastic 3rd-party SDK, packed with features your users will love, but wait! there's no Flutter plugin in sight.


You check pub.dev, GitHub, and even the vendor's documentation… nothing. It only supports **native iOS (Swift/Obj-C)** or **Android (Kotlin/Java)**.
Panic starts to set in, now what?

Flutter has grown a lot, and its ecosystem offers a wide variety of packages to handle most common use cases: camera, sensors, maps, in-app purchases, you name it. But when you run into a situation where you must integrate a native SDK—and there’s no Dart support—it’s easy to feel stuck.

That’s where **[Platform Channels](https://docs.flutter.dev/platform-integration/platform-channels)** come in to save the day.

---

## 🧠 The Problem

Let’s say you’re building an app in Flutter (Dart), but you need to integrate an SDK that’s only available for iOS (Swift). For example, a biometric security vendor, a proprietary device scanner, or a special AR module.

Unfortunately, Flutter doesn’t magically understand Swift or Kotlin. So, how do you make Dart and native code talk to each other?

---

## 💡 The Solution: Platform Channels

Platform Channels are Flutter's official way to bridge communication between Dart and the native side of your app (Swift for iOS, Kotlin for Android).

Here’s the core idea:

- You send a message from Dart using a **MethodChannel**.
- On the native side, you register a method call handler that listens for those messages and returns results.

It's like a walkie-talkie between Dart and Swift.

---

## 🛠️ A Basic Example

Let’s say you want to call a native Swift function from Dart to get the battery level (a classic).

### Dart side (Flutter)

```dart
import 'package:flutter/services.dart';

class BatteryService {
  static const platform = MethodChannel('battery_channel');

  Future<int?> getBatteryLevel() async {
    try {
      final int batteryLevel = await platform.invokeMethod('getBatteryLevel');
      return batteryLevel;
    } on PlatformException catch (e) {
      print("Failed to get battery level: '${e.message}'.");
      return null;
    }
  }
}
```


### iOS side (Swift)

Inside your AppDelegate.swift file:

```swift
import UIKit
import Flutter

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  private let channelName = "battery_channel"

  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {

    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController
    let batteryChannel = FlutterMethodChannel(name: channelName, binaryMessenger: controller.binaryMessenger)

    batteryChannel.setMethodCallHandler({
      (call: FlutterMethodCall, result: FlutterResult) -> Void in
      if call.method == "getBatteryLevel" {
        self.receiveBatteryLevel(result: result)
      } else {
        result(FlutterMethodNotImplemented)
      }
    })

    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }

  private func receiveBatteryLevel(result: FlutterResult) {
    let device = UIDevice.current
    device.isBatteryMonitoringEnabled = true
    if device.batteryState == .unknown {
      result(FlutterError(code: "UNAVAILABLE",
                          message: "Battery info unavailable",
                          details: nil))
    } else {
      result(Int(device.batteryLevel * 100))
    }
  }
}
```

## 🧪 Testing the Integration

Now in your Flutter app, you can use:


```dart
final batteryLevel = await BatteryService().getBatteryLevel();
print("Battery Level: $batteryLevel%");

```

And voilà—your Dart code is talking directly to Swift.

## ✨ When to Use Platform Channels

Use Platform Channels when:

- You need to integrate native-only SDKs.

- You need access to a platform-specific API that has no Flutter equivalent.

- You're prototyping or bridging experimental features that only exist in Swift/Java/Kotlin.

## 🧭 Final Thoughts

Flutter is powerful, but it doesn’t live in isolation. Real-world apps often require native capabilities. When that time comes, don’t panic, **Platform Channels** are your escape hatch.

They allow you to keep your app in Dart/Flutter while still accessing everything native platforms can offer.

Next time you’re stuck trying to integrate something native in Flutter, remember: it’s just one channel away.















