# Blesh React Native SDK 5 Developer Guide

**Version:** *1.2.3*

This document describes integration of the Blesh SDK with your React Native application.

## Table of Contents

- [Blesh React Native SDK 5 Developer Guide](#blesh-react-native-sdk-5-developer-guide)
  - [Table of Contents](#table-of-contents)
  - [Changelog](#changelog)
  - [Introduction](#introduction)
  - [Requirements](#requirements)
    - [Android Requirements](#android-requirements)
    - [iOS Requirements](#ios-requirements)
  - [Integration](#integration)
    - [1. Adding the Blesh React Native SDK NPM library](#1-adding-the-blesh-react-native-sdk-npm-library)
    - [2. (Optional) Linking the Blesh React Native SDK NPM library](#2-optional-linking-the-blesh-react-native-sdk-npm-library)
    - [3. (Optional) Preparing the Development Environment](#3-optional-preparing-the-development-environment)
    - [4. Defining the Android SDK Location](#4-defining-the-android-sdk-location)
    - [5. Managing Native Dependencies](#5-managing-native-dependencies)
      - [Adding Blesh Android SDK Maven Repository](#adding-blesh-android-sdk-maven-repository)
      - [Updating CocoaPods for iOS](#updating-cocoapods-for-ios)
    - [6. Configuring Blesh Android SDK](#6-configuring-blesh-android-sdk)
      - [Secret Key](#secret-key)
      - [Notification Icon](#notification-icon)
      - [Notification Color](#notification-color)
      - [Permissions](#permissions)
    - [7. Configuring Blesh iOS SDK](#7-configuring-blesh-ios-sdk)
      - [Secret Key](#secret-key-1)
      - [Frameworks](#frameworks)
      - [Supporting Files](#supporting-files)
      - [Permissions](#permissions-1)
  - [Usage](#usage)
    - [Configuring the Blesh React Native SDK](#configuring-the-blesh-react-native-sdk)
    - [Starting the Blesh React Native SDK](#starting-the-blesh-react-native-sdk)
    - [Notifying the Blesh iOS SDK About Push Notifications](#notifying-the-blesh-ios-sdk-about-push-notifications)
    - [Controlling Android Push Notification Campaign Rendering](#controlling-android-push-notification-campaign-rendering)
    - [Getting Notified When a Blesh Campaign is Displayed on Android](#getting-notified-when-a-blesh-campaign-is-displayed-on-android)
    - [Notifying the Blesh iOS SDK About Changes in Permissions](#notifying-the-blesh-ios-sdk-about-changes-in-permissions)

## Changelog

  * **1.2.3** *(Released 2022-11-17)*
    * Released with Blesh Android SDK v5.4.4 and Blesh iOS SDK v5.4.5

  * **1.2.2** *(Released 2022-08-26)*
    * Removed a dependency on the maven plugin for Gradle 7 compatibility

  * **1.2.1** *(Released 2022-08-26)*
    * Released with Blesh Android SDK v5.4.3 and Blesh iOS SDK v5.4.2

  * **1.2.0** *(Released 2022-07-19)*
    * Released with Blesh Android SDK v5.4.2 and Blesh iOS SDK v5.4.0

  * **1.1.0** *(Released 2022-04-21)*
    * Released with Blesh Android SDK v5.3.0 and Blesh iOS SDK v5.3.0

  * **1.0.1** *(Released 2022-04-13)*
    * Updated build.gradle

  * **1.0.0** *(Released 2022-03-23)*
    * Initial release with Blesh Android SDK v5.2.8 and Blesh iOS SDK v5.2.10

## Introduction

Blesh SDK collects location information from a device on which the React Native application is installed. Blesh Ads Platform uses this data for creating and enhancing audiences, serving targeted ads, and insights generation.

## Requirements

This library is tested using React Native v0.67.3.

### Android Requirements

Compile SDK Version and Target SDK Version of Blesh Android SDK is 31. In order to integrate the Blesh Android SDK make sure you are:

  * Targeting Android version 4.1 (API level 16) or higher
  * Registered on the [Blesh Publisher Portal](https://publisher.blesh.com)
    * You may need to create a *Blesh Ads Platform Access Key* for the Android platform

> **Note:** BleshSDK uses AndroidX libraries. You may need to use the "Migrate to AndroidX" feature in your IDE and add the definitions below to your gradle.properties file of your project.

```groovy
android.enableJetifier=true
android.useAndroidX=true
```

Please refer to [Migrating to AndroidX](https://developer.android.com/jetpack/androidx/migrate) for more information.

### iOS Requirements

In order to integrate the Blesh iOS SDK make sure you are:

  * Targeting iOS version 9 or higher
  * Targeting the Swift 5 compiler
  * Enabling the "`Always Embed Swift Standard Libraries`" build option (or the "`Embedded Content Contains Swift Code`" build option for older versions of Xcode) if your application is developed using the Objective-C language
    * Swift Standard Libraries are required for iOS versions 12.1 or earlier. See [QA1881](https://developer.apple.com/library/archive/qa/qa1881/_index.html) for details
  * Registered on the [Blesh Publisher Portal](https://publisher.blesh.com)
    * You may need to create a *Blesh Ads Platform Access Key* for the iOS platform

> **Note:** Make sure that you declare that "your app uses IDFA" on App Store Connect. Otherwise, your app may be rejected on review. Blesh iOS SDK collects IDFA from the device, in full compliance with the Apple [requirements](https://support.apple.com/en-us/HT205223).

## Integration

### 1. Adding the Blesh React Native SDK NPM library

NPM registry for "@blesh" scope can be defined as:

```shell
npm config set @blesh:registry https://artifact.blesh.com/repository/npm/
```

After this one-time definition, the npm library can be added with:

```shell
npm install "@blesh/blesh-react-native-sdk" --save
```

### 2. (Optional) Linking the Blesh React Native SDK NPM library

> This step is not required with newer versions of React Native

If you use npx then you can link the library using:

```shell
npx react-native link @blesh/blesh-react-native-sdk
```

Alternatively, if you use react native CLI then you can link the library using:

```shell
react-native link @blesh/blesh-react-native-sdk
```

### 3. (Optional) Preparing the Development Environment

If you haven't already done so, you can use `expo` to eject native source code:

```shell
expo eject
``` 

[This site](https://docs.expo.dev/workflow/customizing/) contains more information about the process.

Native Android module will be located under the `android` subfolder. Similarly, native iOS module will be located under the `ios` subfolder.

### 4. Defining the Android SDK Location

Add following line to `android/local.properties` file to define the Android SDK location.

```ini
sdk.dir = /path/of/your/android/sdk
```

Alternatively, building the Android application with Android Studio may also set the SDK location.

### 5. Managing Native Dependencies

#### Adding Blesh Android SDK Maven Repository

Add BleshSDK Maven repository to `android/build.gradle` file.

```groovy
allprojects {
  repositories {
      // ... existing repositories ...

      maven { url 'https://artifact.blesh.com/repository/releases/' }
  }
}
```

#### Updating CocoaPods for iOS

Install pods by running the following command on the terminal:

```bash
pod install
```

### 6. Configuring Blesh Android SDK

#### Secret Key

Blesh Android SDK requires **Blesh Ads Platform Access Key**. You may need to create one for the Android platform at the [Blesh Publisher Portal](https://publisher.blesh.com). If you do not have an account at the [Blesh Publisher Portal](https://publisher.blesh.com) please contact us at technology@blesh.com.

If you have your key you can provide it in your `AndroidManifest.xml` file with the `com.blesh.sdk.secretKey` key.

#### Notification Icon

Push notifications are rendered with the Blesh logo by default. You can customize this logo by providing a resource with the `com.blesh.sdk.notificationIcon` key.

#### Notification Color

Push notifications are rendered with the `#351F78` color by default. You can customize this color by providing a resource with the `com.blesh.sdk.notificationColor` key.

#### Permissions

In order to properly initialize the SDK, you need to use internet and access network/wifi state permissions.

**Example manifest file:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.your.application">

    <!-- ... -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" tools:node="replace" />
    <uses-permission android:name="android.permission.BLUETOOTH" android:maxSdkVersion="30" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" android:maxSdkVersion="30" />
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN" />

    <uses-feature android:name="android.hardware.bluetooth" android:required="false"/>
    <uses-feature android:name="android.hardware.bluetooth_le" android:required="false" />

    <!-- ... -->

    <application
        android:name=".YourApplication">
        <!-- ... -->

        <!-- Secret key -->
        <meta-data
            android:name="com.blesh.sdk.secretKey"
            android:value="YOUR-SECRET-KEY" />

        <!-- Custom notification icon -->
        <meta-data
            android:name="com.blesh.sdk.notificationIcon"
            android:resource="@drawable/compatibleIcon" />

        <!-- Custom notification color -->
        <meta-data
            android:name="com.blesh.sdk.notificationColor"
            android:resource="@color/colorPrimary" />

    </application>

</manifest>
```

### 7. Configuring Blesh iOS SDK

#### Secret Key

Blesh iOS SDK requires the **Blesh Ads Platform Access Key**. You may need to create one for the iOS platform at the [Blesh Publisher Portal](https://publisher.blesh.com). If you do not have an account at the [Blesh Publisher Portal](https://publisher.blesh.com) please contact us at technology@blesh.com. 

You can set your key in your application's `Info.plist` file with the `Blesh Platform Access Key` key:

```xml
<key>Blesh Platform Access Key</key>
<string>YOUR_SECRET_KEY_HERE</string>
```

#### Frameworks

Blesh iOS SDK utilizes following frameworks. Please make sure that your project references all of them:

 * Foundation.framework
 * UIKit.framework
 * AdSupport.framework
 * CoreLocation.framework
 * CoreTelephony.framework
 * SystemConfiguration.framework

#### Supporting Files

Blesh iOS SDK plays its default sound file `BleshNotification.caf` for supported ads. You can download the sound file from [this link](https://github.com/bleshcom/Blesh-iOS-SDK/blob/master/Supporting%20Files/BleshNotification.caf?raw=true) and copy into your application's `Supporting Files` folder. If this file doesn't exist then the SDK plays the default iOS notification sound.

#### Permissions

In order to provide proper notifications and proper beacon tracking, you need to get some permission from the application user, after your application is installed. Per iOS User guides, applications can ask for required permissions with their own sentences. The permissions can be configured in `Info.plist` file.

Blesh iOS SDK uses iBeacon protocol which requires location permission by default. Blesh iOS SDK needs to detect beacons and geofences even when the app is at background or killed. Therefore you have to ask for *"Always usage of location"* to your users.

Until iOS 11, when the users give permission for location usage, it was considered as *"Always"* and applications could use the location when they are killed or at background.

After iOS 11, the rules have changed and users were able to give location usage permission only when the application is in use. As a result, for iOS v11 and later, applications have to ask for two different types of location permissions: `WhenInUse` or `AlwaysAndWhenInUse`.

For backward compatibility, your application should include all three descriptors given below for location permission:

 * NSLocationAlwaysAndWhenInUseUsageDescription
 * NSLocationWhenInUseUsageDescription
 * NSLocationAlwaysUsageDescription

In the description field of location permission entries, we recommend you to encourage your users to allow *"Always use my location"*, in order to have a better performance of the system. In the below subsections, we provide some sample description text. Please check and consider them.

Please note that, with iOS version 11, applications must include all of the below 3 descriptors in their `Info.plist` file for backward compatibility with older OS versions.

1. **Always And When In Use**

This descriptor is used for iOS 11 and later. Provides permission for both when in use and when killed.

Sample Text: *"This application uses your location in order to inform you about interesting offers nearby. We advice you to choose Always option to get the offers even when you are not using the application!"*

You can insert it into your `Info.plist` file in the following syntax.

```xml
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string><# insert always and when in use usage description text #></string>
```

2. **Always**

This descriptor is used for iOS 10 and before versions. Provides permission for both when in use and when killed.

Sample Text: *"This application uses your location in order to inform you about interesting offers nearby. We advice you to choose Always option to get the offers even when you are not using the application!"*

You can insert it into your `Info.plist` file in the following syntax.

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string><# insert always usage description text #></string>
```

3. **When In Use**

This descriptor is used in all iOS versions. Provides permission for only when the application is in use. We advice you to warn your users about very low performance on receiving nearby offers.

Sample Text: *“This application uses your location in order to inform you about interesting offers nearby. Allowing location when in use only may result in poor performance in finding nearby offers!”*

You can insert it into your `Info.plist` file in the following syntax.

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string><# insert when in use usage description text #></string>
```

## Usage

You can import `BleshSdk` in either `index.js` or `App.js` file.

```javascript
import BleshSdk from '@blesh/blesh-react-native-sdk';
```

### Configuring the Blesh React Native SDK

Blesh SDK contains following overrides of the configure method:

```javascript
BleshSdk.configure({});
```

> Configuration parameter allows you to configure the behaviour of the Blesh SDK. The configuration object contains the following:

| Property   |	Type   | Description                                                                       | Example |
|------------|---------|-----------------------------------------------------------------------------------|---------|
| adsEnabled | boolean | Configures getting ads from SDK                                                   | `true`    |
| testMode   | boolean | Use the SDK in the test mode (true) or use the SDK in the production mode (false) | `false`   |

> Note: TestMode is off by default. You can enable this mode during your integration tests. Production environment will not be effected when this flag is set to true.

**Example:** Simple Configuration

You can configure the Blesh SDK by simply providing the application entry point of your React Native application:

```javascript
import BleshSdk from '@blesh/blesh-react-native-sdk'

export default function App() {
  BleshSdk.configure({"adsEnabled": true});

  // ...
}
```

### Starting the Blesh React Native SDK

After configuring Blesh SDK, you can start in one of ReactNative Component.

**Example:** Simple Start

```javascript
export default function App() {
  BleshSdk.start({
    "userId": "42",
    "email": "jane.doe@example.com",
    "gender": 0,
    "yearOfBirth": 1999
  })
  .then((state) => console.log("State: " + state))
  .catch((code) => console.log(code));

  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

Start method takes one parameter: `ApplicationUser` object. That object contains properties following

| Property    | Type                   | Description                                        | Example                       |
|-------------|------------------------|----------------------------------------------------|-------------------------------|
| userId      | String                 | Optional unique identifier of the user             | `42`                            |
| gender      | Integer                | Optional gender of the user (0: Female, 1: Male)   | `0`                             |
| yearOfBirth | Integer                | Optional year of birth of the user                 | `1999`                          |
| email       | String                 | Optional email address of the user                 | `jane.doe@example.com`          |
| phoneNumber | String                 | Optional mobile phone number of the user           | `+905550000000`                 |
| other       | Dictionary             | Optional extra information for the user            | `{ "Country": "TR" }`           |

### Notifying the Blesh iOS SDK About Push Notifications

Blesh iOS SDK **must be** notified when a push notification is about to be displayed and when a user interacted with a push notification. This will allow Blesh iOS SDK to:

- Display a notification when the application is in the foreground (iOS 10+)
- Display an ad when the user taps a notification

**Example:** Native Swift code

```swift
import UIKit
import UserNotifications
import BleshSDK

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // mark this class as a UNUserNotificationCenterDelegate
        if #available(iOS 10, *) {
            UNUserNotificationCenter.current().delegate = self
        }

        // handle any new not-processed notifications
        if let notification = launchOptions?[UIApplication.LaunchOptionsKey.localNotification] as? UILocalNotification {
            BleshSdk.sharedInstance.didReceiveLocalNotification(notification)
        }

        // ... rest of the method ...

        return true
    }

    // this method will be called when app received push notifications in foreground
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void)
    {
        completionHandler([.alert, .badge, .sound])
    }

    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        BleshSdk.sharedInstance.didReceiveUNNotificationResponse(response)
        completionHandler()
    }

    // compatibility with iOS < 10
    func application(_ application: UIApplication, didReceive notification: UILocalNotification) {
        BleshSdk.sharedInstance.didReceiveLocalNotification(notification)
    }

    // compatibility with iOS < 10
    func application(_ application: UIApplication, handleActionWithIdentifier identifier: String?, for notification: UILocalNotification, completionHandler: @escaping () -> Void) {
        BleshSdk.sharedInstance.didReceiveLocalNotification(notification)
    }

    // ... rest of the class ...
}
```

**Example:** Native Objective-C code (AppDelegate.h)

```objective-c
#import <UserNotifications/UserNotifications.h>
#import <CoreLocation/CoreLocation.h>
// ... rest of imports ...

@interface AppDelegate : EXAppDelegateWrapper <RCTBridgeDelegate, UNUserNotificationCenterDelegate>

// ... rest of the interface ...

@end
```

**Example:** Native Objective-C code (AppDelegate.m)

```objective-c
#import <BleshSDK/BleshSDK-Swift.h>

// ... rest of imports ...

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  // mark this class as a UNUserNotificationCenterDelegate
  if (@available(iOS 10, *)) {
    [[UNUserNotificationCenter currentNotificationCenter] setDelegate:self];
  }

  // ... rest of the method ...

  return YES;
 }

// this method will be called when app received push notifications in foreground
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
       willPresentNotification:(UNNotification *)notification
         withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
{
  completionHandler(UNNotificationPresentationOptionAlert | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)(void))completionHandler
{
  [[BleshSdk sharedInstance] didReceiveUNNotificationResponse:response];

  completionHandler();
}

// ... rest of the class ...

@end
```

### Controlling Android Push Notification Campaign Rendering

```javascript
if (Platform.OS === 'android') {
  BleshSdk.setOnCampaignNotificationReceived(e => {
    console.log("campaignId:" + e.campaignId);

    // set to deny or accept Blesh SDK from displaying this campaign and push notification
    BleshSdk.abortCampaignNotification(e.campaignId)
  });
}
```

### Getting Notified When a Blesh Campaign is Displayed on Android

```javascript
if (Platform.OS === 'android') {
  BleshSdk.setOnCampaignDisplayed(e => {
    console.log("campaignId:" + e.campaignId);
    console.log("contentId:" + e.contentId);
    console.log("notificationId:" + e.notificationId);
  });
}
```
### Notifying the Blesh iOS SDK About Changes in Permissions

Starting from Blesh iOS SDK 4.0.7, the SDK does not ask the user for permissions. Your application needs to ask for location permissions. See "[iOS Permissions](#permissions-1)" section for more information.

When the location permission changes, your application should call the `didChangeLocationAuthorization` method of `BleshSdk` with the new status.

**Example:** Native Swift code

```swift
import UIKit
import BleshSDK
import CoreLocation

class MyViewController: UIViewController, CLLocationManagerDelegate {
    var locationManager: CLLocationManager

    required init?(coder aDecoder: NSCoder) {
        self.locationManager = CLLocationManager()

        super.init(coder: aDecoder)
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        self.locationManager.delegate = self
        self.locationManager.desiredAccuracy = kCLLocationAccuracyBest
        self.locationManager.distanceFilter = 10
        self.locationManager.requestAlwaysAuthorization()
        self.locationManager.requestLocation()

        // ... rest of the method ...
    }

    public func locationManager(_ manager: CLLocationManager, didChangeAuthorization status: CLAuthorizationStatus) {
        // Notify the Blesh iOS SDK about the change here
        BleshSdk.sharedInstance.didChangeLocationAuthorization(status)
    }

    // ... rest of the controller ...
}
```
