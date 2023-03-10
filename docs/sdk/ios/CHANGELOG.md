---
layout: default
title: iOS Changelog
parent: iOS Integration
nav_order: 3
---

Please note: in order to update to a new version, write these command:
- pod deintegrate
- pod cache clean --all
- pod install


# Phoenix iOS SDK Changelog

## 1.4.4
* Add support for XCode 14.2;

To use this version define the pod with PhoenixSdk_{VERSION}: 

for device: 

```swift
pod "PhoenixSdk_1.4.4", :http => 'https://nextome.jfrog.io/artifactory/nextome-cocoapods-local/nextome-sdk.tar.gz', :type => 'tgz' 
```

for simulator:

```swift
pod "PhoenixSdk_1.4.4_x86", :http => 'https://nextome.jfrog.io/artifactory/nextome-cocoapods-local/nextome-sdk.tar.gz', :type => 'tgz' 
```


## 1.4.3
* Add support for XCode 13.3;

## 1.4.0
* Add support for events

You can observe onEnter and onExit from event radius on map using those two observers:
```swift

NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveEventEnter(_:)), name: NSNotification.Name(rawValue: "EVENT_ENTER_STREAM"), object: nil)
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveEventExit(_:)), name: NSNotification.Name(rawValue: "EVENT_EXIT_STREAM"), object: nil)
        
```
Optionally, after exiting from event, it's possible to set a timeout value in seconds before signaling that event again.
If set to 0, Nextome SDK will signal onEnter and onExit event realtime.

```swift
   self.sdk = NextomeSdk(eventTimeout: 10)
```

## 1.3.2
* getVersion()


## 1.3.1
* Minor Fix
* New Method getVenueResource

[Read Here](https://docs.nextome.dev/sdk/ios/README.html#venue-resources-sdk-v--131)




## 1.3.0
* Update BLE ranging feature
* Dependency update

## 1.2.0
* Fix sdk log with utc time standard
