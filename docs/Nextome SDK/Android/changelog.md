---
layout: default
title: Android Changelog
parent: Android Integration
nav_order: 2
hide:
  - footer
---

# Nextome Android SDK - Changelog
## 0.4.1
### Bugfix
 * Fix issue with timestamps when sending live positions;

## 0.4.0
### Features
* Add support for events;

You can observe onEnter and onExit from event radius on map using those two observers:
```kotlin
    nextomeSdk.enterEventObservable.observe(this) { event ->
        Log.d("event_test", "Received on enter with data: ${event.data}")
    }

    nextomeSdk.exitEventObservable.observe(this) { event ->
        Log.d("event_test", "Received on exit with data: ${event.data}")
    }
```


Optionally, after exiting from event, it's possible to set a timeout value in seconds before signaling that event again.
If set to 0, Nextome SDK will signal onEnter and onExit event realtime.
```kotlin
    nextomeSdk = NextomePhoenixSdk().Builder(applicationContext)
        .withEventTimeoutDurationInSeconds(10L)
```

## 0.3.1
### Bug fixing
* Fix bug when the correct bundle was not injected in request headers correctly;
* Throw descriptive exception when developer key, secret or bundle are not added to SDK;

## 0.3.0
### Features
* Add method to get venue resources;

In observer:
```kotlin
    nextomeSdk.stateLiveData.observe(this, {
        when (it.state) {
            NextomePhoenixState.RUNNING -> {
                val poiList = it.venueResources.allPois
                val poiOfTheCurrentFloor = it.venueResources.getPoisByMapId(it.mapId)
                val mapsInTheVenue = it.venueResources.maps
            }
            else -> { }
        }
    })
```

As a single method call:
```kotlin
    val resources = nextomeSdk.state.venueResources
```

*Note that you can only retreive resources when Nextome SDK State is RUNNING. Otherwhise, `nextomeSdk.state.valueResources` will return a null object.*

 * Add method to get current sdk state;
```kotlin
    val currentState = nextomeSdk.state 
```

### Deprecated methods
* `nextomeSdk.currentState.asLiveData()` is now deprecated. Use `nextomeSdk.stateLiveData` instead;
* `state.poisOfMap` is now deprecated. Use `state.venueResources.getPoisByMapId(mapId)` instead;
* `state.allPois` is now deprecated. Use `state.venueResources.getAllPois()` instead;


## 0.2.8
* Add method to send locations to server:
 ```kotlin
NextomePhoenixSdk().Builder(applicationContext)
    .withSendPositionToServer(true)
 ```

## 0.2.7
* Fix issue in Event's radius parser;
* Add debug tools to log on file: `startLoggingOnFile()` and `stopAndShareLog(context)`

## 0.2.2
* Expose current map POIs in observer;

## 0.2.1
* Add new `setForcedMap` and `setLiveMap` methods.
* Improvements to Flutter map;

## 0.1.2
* Fix issue with floor and map Id;
* Fix issue with outdoor state;

## 0.1.1
* Added mapId and floorId in NextomePosition.

You can now listen for map and floor changes using the localizationLiveData.
 ```kotlin
         nextomeSdk.localizationLiveData.observe(this, Observer {
            val floor = it.floorId
            val map = it.mapId
            log( "User Position is ${it.x}, ${it.y}")
        })
 ```
## 0.1.0
* Initial release;