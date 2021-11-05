---
layout: default
title: Android Changelog
parent: Android Integration
nav_order: 2
---

# Phoenix Andorid SDK Changelog

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
