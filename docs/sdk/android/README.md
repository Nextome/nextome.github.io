# Nextome Phoenix SDK
![Nextome Android Sdk Image](https://raw.githubusercontent.com/Nextome/nextome-phoenix-android-whitelabel/main/resources/img-sdk-githubBx2.png?token=ABK44BJ4J33AED564QB5BCLBGCIPK)

# How to include
1. Add our repositories in root build.gradle:
```gradle
  allprojects {
    repositories {
      ...
      maven { url "http://production.nextome.net:8082/artifactory/libs-release-local" }
      maven { url 'https://jitpack.io' }
    }
  }
```


2. Add SDK dependency in app's build.gradle:

Check latest released version [here](CHANGELOG.md)
```gradle
    implementation('net.nextome.phoenix_sdk:phoenix-sdk:{last_version}')
```

### Optional Dependencies
If you plan to display a live map to the user, add our Flutter MapView plugin:

1. Add Flutter repository in root build.gradle:
```gradle
  allprojects {
    repositories {
      ...
      maven {url 'https://storage.googleapis.com/download.flutter.io' }
    }
  }
```


2. Add SDK dependency in app's build.gradle:
```gradle
    implementation('net.nextome.nextome_map_module:flutter_release:1.1.0')
```

# Getting started

## Whitelabel app
A complete example of an Android App build on top on Nextome's SDK is available [here](https://github.com/Nextome/nextome-phoenix-android-whitelabel).

## Required permissions
To run, Nextome SDK requires the following permissions:
```
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

Those are required to:
- Contact the backend server;
- Activate and manage Bluetooth beacon scan;
- Run as a foreground service for background mode;
- Access Bluetooth API (location)

## SDK Builder
To use Nextome SDK for Android, a builder is available with some options you can customize.

The basic configuration only needs your application `context`, the given `developer key` and the `secret`.

```kotlin
nextomeSdk = NextomePhoenixSdk().Builder(applicationContext)
                .withSecret(secret)
                .withDeveloperKey(developerKey)
                .build()
```
Other **optional** parameters are also available. Here's a list of the customization available and a short description of them.

### Scanner parameters

You can optionally customize scan period and scan duration.

> The longer you wait between scans, the longer it will take to detect a beacon. And the more reduce the length of the scan, the more likely it is that you might miss an advertisement from an beacon.
___
```kotlin
.withForegroundScanPeriod(scanPeriod)`
```

sets a time in millis for the bluetooth beacon scan duration when in foreground mode.
> Default value is `1000 ms`
___
```kotlin
.withForegroundBetweenScanPeriod(betweenScanPeriod)`
```

sets a time in millis to wait between bluetooth scans when in foreground mode.

> Default value is `250 ms`.
___
```kotlin
.withBackgroundScanPeriod(scanPeriod)`
```

sets a time in millis for the bluetooth beacon scan duration when in backgorund mode.

> Default value is `1000 ms`
___
```kotlin
.withBackgroundBetweenScanPeriod(betweenScanPeriod)`
```

sets a time in millis to wait between bluetooth scans when in background mode.

> Default value is `250 ms`.
___
### Other optional parameters
```kotlin
.withRssiThreshold(rssiThreshold)`
```

ignore beacons below a custom RSSI threshold.
> Default value is `-75`.
___
```kotlin
.withBeaconListMaxSize(n)`
```

keeps in memory a cache of `n` RSSI entries in time for each beacon to more accurately compute user position.

> Default value is `12`
___
```kotlin
.withLocalizationMethod(localizationMethod)`
```

Sets the algorithm for localization. Choises are `LINEAR_SVD, NON_LINEAR_DA, PARTICLE`.

> Default value is `LINEAR_SVD`

___
```kotlin
.withForcedVenue(venueId)
```

This allows you to skip the search for venue phase and automatically download resurces of a specific venue when Nextome starts.

> With this, you can force your app to open only your venue and download venue resurces even if you're not in range of your venue beacons.
___
```kotlin
.withSendPositionToServer(enabled)
```

This allows you to automatically send positions to Nextome server and track monitor devices realtime on the web manager.

> Default value is `false`

## Start localization
To start localization, call:

```kotlin
nextomeSdk.start()
```

If you want Nextome to keep track of user indoor position also while the phone screen is off or your app is in background, call this method instead:

```kotlin
nextomeSdk.startForegroundService(serviceCode, notification)
```

In this case, a `service code` and a `notification` is also required. The ongoing notification will be displayed while Nextome SDK runs in background mode.

## Observe user position
Nextome SDK offers two different observers to notify user position.

- `nextomeSdk.localizationLiveData` will notify indoor user position,with info about:
 - `x`, `y` coordinates of the user relative to the indoor map;
 - `mapId` and `floorId` of the current user position;
- `nextomeSdk.outdoorLocalizationLiveData` will notify outdoor user position, with its `latitude` and `longitde`.

Usage example:
```kotlin
nextomeSdk.localizationLiveData.observe(this, {
    log("Got indoor position (${it.x}, ${it.y}), on map ${it.mapId} in floor ${it.floorId})")
})

nextomeSdk.outdoorLocalizationLiveData.observe(this, {
    log("Got outdoor position (${it.lat}, ${it.lng})")
})
```

## Observe SDK status
It's possibile to observe the current state the Nextome SDK, in the localization process.

You can use this data to start initializing the map or showing messages to the users and update your UI accordingly.

```kotlin
nextomeSdk.currentState.asLiveData().observe()
```

The exposed information are:
- `isOutdoor`: indicates if the SDK is running in outdoor or indoor mode;
- `tiles`: local path on device of map tiles. You can use this info to initialize a map (for example, our Flutter map) to show the map to the user;
- `mapHeight`: the height in pixel of the currentmap;
- `mapWidth`: the width in pixel of the current map;
- `poisOfMap`: Points of interest on the current floor;
- `allPois`: Point of interests of all maps;
- `state` Nextome SDK state.

### Nextome SDK State
Nextome State is a simple state machine that can have those states:
- `STARTED`: Nextome has been correctly initialized and it's ready to scan beacons;
- `SEARCH_VENUE`: Nextome is currently scanning nearby beacons to determine in which venue the user is; If the SDK is stuck here, you're probably outdoor.
- `GET_PACKET`: Nextome knows the venue of the user and it's downloading from the server
  the associated resources (Maps, POIs, Patches...);
- `FIND_FLOOR`: All the venue resoruces have been downloaded. Nextome is now computing in which floor the user is;
- `RUNNING`: Nextome SDK is computing user positions. You can observe live user location using the observer nextomeSdk.locationLiveData;

Notes:
- Map `tiles`, `poisOfMap`, `pois`, `height` and `width` are only available in the `RUNNING` state (when Nextome has recognized or has been forced the venue and the map where the user is);
- If the user **changes floor**, the SDK will resume from `FIND_FLOOR` state.
- If the user goes **outdoor**, the SDK will switch to `SEARCH_VENUE` state until a new indoor beacon is detected.


#### Example
```kotlin
nextomeSdk.currentState.asLiveData().observe(this, {
    if (it.isOutdoor) {
        showOpenStreetMap()
    } else {
        showIndoorMap()
    }

    when (it.state) {
        NextomePhoenixState.STARTED -> {
            updateState("Sdk Started")
        }
        NextomePhoenixState.SEARCH_VENUE -> {
            updateState("Searching Venue...")
        }
        NextomePhoenixState.GET_PACKET -> {
            updateState("Downloading packet...")
        }
        NextomePhoenixState.FIND_FLOOR -> {
            updateState("Finding current Floor...")
        }
        NextomePhoenixState.RUNNING -> {
            updateState("Loading map...")

            with(it) {
                setIndoorMap(mapTilesUrl, mapHeight, mapWidth, poisOfMap)
            }
        }
    }
})
```

### Observe SDK errors
You can be notified of SDK errors using the appropriate observer:
```kotlin
nextomeSdk.errorObservable.observe(this) {
    notifyException(it.exception)
}
```

### Force a specific map
This is expecially useful when using the Flutter Map Module.

With this method you can force Nextome to show a specific map, indipendently from the user real indoor location.

> When this method is called, Nextome will stop sending indoor location updates.

You can force a map using:
```kotlin
nextomeSdk.setForcedMap(mapId)
```

Nextome will immediatly fire a status change, localizing the user at the specified map and offering map information like `tiles`, `height` and `width` that you can pass to the map module.

When you're done and you want to observe user real location again, call:
```kotlin
nextomeSdk.setLiveMap()
```

#### Example
```kotiln
nextomeSdk.currentState.asLiveData().observe(this, {
    when (it.state) {
        NextomePhoenixState.RUNNING -> {
            with(it) {
                // After forcing the map, here you can pick tiles, height and width of map 42.

                setIndoorMap(mapTilesUrl, mapHeight, mapWidth)
            }
        }
    }
})

// Force map 42
nextomeSdk.setForcedMap(42)
```

## Calcualte path
Nextome Sdk is able to calculate the shortest path between two points of the same venue.

The function takes (x, y) coordinates and map of the starting point and (x, y) coordinates and map of the destination point.

```kotlin
val path = nextomeSdk.findPath(
                startX.toInt(), startY.toInt(), startMapId,
                targetX.toInt(), targetY.toInt(), targetMapId)
```

The returned path is a list of ordered Vertex, that, for example, can be passed to the Flutter Map Module and displayed to the user (see the methods writtenbelow).

## Flutter Map Module
If you want, there's an optional module build with Flutter that can show a live map of the indoor location of the user.

To use it, include the gradle dependency as written before in the *How to include* section.

### Flutter Init

A little of boilerplate code is required to initialize and set up a channel between Flutter and the native app.

You can just paste this code and run it before showing the map to the user.

```kotlin
private fun initFlutter() {
    val appId = "com.nextome.test"
    
    val engine = FlutterEngine(this)

    // Start executing Dart code in the FlutterEngine.
    engine.dartExecutor.executeDartEntrypoint(
        DartExecutor.DartEntrypoint.createDefault()
    )

    // Cache the pre-warmed FlutterEngine to be used later by FlutterFragment.
    FlutterEngineCache
        .getInstance()
        .put(appId, engine)

    val fragmentManager: FragmentManager = supportFragmentManager

    // Attempt to find an existing FlutterFragment, in case this is not the
    // first time that onCreate() was run.
    flutterFragment = fragmentManager
        .findFragmentByTag(TAG_FLUTTER_FRAGMENT) as FlutterFragment?


    // Create and attach a FlutterFragment if one does not exist.
    if (flutterFragment == null) {
        var newFlutterFragment = FlutterFragment
            .withCachedEngine(appId).build<FlutterFragment>()

        flutterFragment = newFlutterFragment

        fragmentManager
            .beginTransaction()
            .add(
                R.id.indoor_map,
                newFlutterFragment,
                TAG_FLUTTER_FRAGMENT
            ).commit()
    }

    channel = MethodChannel(engine.dartExecutor, appId)
}
```

### Flutter Utils
An utility class is available to help you with Flutter operations. If you want to comunicate with Flutter, it's suggested to include [this](https://github.com/Nextome/nextome-phoenix-android-whitelabel/blob/main/app/src/main/java/com/nextome/test/helper/flutter/FlutterUtils.kt) class in your project.
### Show a new indoor map
Once a map is available, pass the `tiles` local directory, height` and `width` of the map to Flutter:

```kotlin
private fun setIndoorMap(mapTilesUrl: String, mapHeight: Int, mapWidth: Int) {
    channel.invokeMethod("localPackageUrl",
        "$mapTilesUrl,$mapHeight,$mapWidth, 3")
}
```
### Update user live position
When a new indoor user position is available, notify Flutter with:
```kotlin
private fun updatePositionOnFlutterMap(it: NextomePosition) {
    channel.invokeMethod("localPackageUrl", FlutterUtils.getLocalPackagePayload(mapTilesUrl, mapHeight, mapWidth))
}
``` 

### Add a POI
You can add custom Point of Interest on the Flutter map using this method:
```kotlin
private fun showPoiOnMap(poiList: List<NextomePoi>) {
    channel.invokeMethod("POI", FlutterUtils.getPoiPayload(poiList))
}
```

### Show a custom patch
You can show a path on the map using this method and passing a list of Vertex. You can automatically get the Vertext between a start and end point of the map using the appropriate method on the `nextomeSdk`:
```kotlin
private fun showPathOnMap(path: List<Vertex>) { 
    channel.invokeMethod("path", FlutterUtils.getPathPayload(path))
}
```

### Observe events on Flutter Map
With this observer you can be notified of events on the Flutter map, for example, when the user decides to navigate to a POI.
```kotlin
private fun observeMapEvents() {
    channel.setMethodCallHandler { methodCall, _ ->
        // On POI "Navigate to" tapped
        if (methodCall.method == "poiData") {
            val poiSerialized = methodCall.arguments as String
            val poi = Gson().fromJson(poiSerialized, FlutterPoi::class.java)

            // Ex. calculate path to POI and show it to user
            showPathOnMap(
                lastPosition.x, lastPosition.y lastPosition.mapId,
                poi.x!!, poi.y!!, poi.map!!)
        }
    }
}
```

## Debug tools
In case of necessity, Nextome SDK can write logs with useful info to a file.
You can start writing logs with `nextomeSdk.startLoggingOnFile()`. Those logs can then be shared using `nextomeSdk.stopAndShareLog(context)`. A new Intent will be fired with a share dialog that allows to export and save the logs with other apps.

# Examples
A full working example app is available on this repository.
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.

<br>

**© 2021 Nextome srl | All Rights Reserved.**
