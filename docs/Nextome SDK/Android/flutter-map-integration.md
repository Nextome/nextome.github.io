# Flutter Map Module
If you want, there's an optional module build with Flutter that can show a live map of the indoor location of the user.

To use it, include the gradle dependency as written before in the *How to include* section.

## How to include
To include the Flutter Map Module inside your project: 

1. Inside the Gradle Project Settings, add the Flutter repository:

    === "Groovy"
        ```groovy title="settings.gradle"
            dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...
                maven {url 'https://storage.googleapis.com/download.flutter.io' }

            }
        }
        ```
    === "KTS"
        ``` kotlin title="settings.gradle.kts"
            dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...
                maven {url = uri("https://storage.googleapis.com/download.flutter.io") }

            }
        }
        ```

2. Add Flutter Map SDK dependency in your module (app-level) Gradle file `build.gradle`

    === "Groovy"
        ```groovy title="app/build.gradle"
        implementation 'net.nextome.nextome_map_module:flutter_release:{last_version}'
        ```

    === "KTS"
        ```kotlin title="app/build.gradle.kts"
        implementation("net.nextome.nextome_map_module:flutter_release:{last_version}")
        ```
    
    Check latest released version [here change me](/docs/Flutter%20Map/index.md)

## Flutter Initialization

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

## Flutter Utils
An utility class is available to help you with Flutter operations. If you want to comunicate with Flutter, it's suggested to include [this](https://github.com/Nextome/nextome-phoenix-android-whitelabel/blob/main/app/src/main/java/com/nextome/test/helper/flutter/FlutterUtils.kt) class in your project.

## Show a new indoor map
Once a map is available, pass the `tiles` local directory, height` and `width` of the map to Flutter:

```kotlin
private fun setIndoorMap(mapTilesUrl: String, mapHeight: Int, mapWidth: Int) {
    channel.invokeMethod("localPackageUrl",
        "$mapTilesUrl,$mapHeight,$mapWidth, 3")
}
```
## Update user live position
When a new indoor user position is available, notify Flutter with:
```kotlin
private fun updatePositionOnFlutterMap(it: NextomePosition) {
    channel.invokeMethod("localPackageUrl", FlutterUtils.getLocalPackagePayload(mapTilesUrl, mapHeight, mapWidth))
}
``` 

## Add a POI
You can add custom Point of Interest on the Flutter map using this method:
```kotlin
private fun showPoiOnMap(poiList: List<NextomePoi>) {
    channel.invokeMethod("POI", FlutterUtils.getPoiPayload(poiList))
}
```

## Show a custom patch
You can show a path on the map using this method and passing a list of Vertex. You can automatically get the Vertext between a start and end point of the map using the appropriate method on the `nextomeSdk`:
```kotlin
private fun showPathOnMap(path: List<Vertex>) { 
    channel.invokeMethod("path", FlutterUtils.getPathPayload(path))
}
```

## Observe events on Flutter Map
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

## Show Center Position Fab
[:octicons-tag-24: 1.2.0]()

It's possible to show an optional fab at the bottom right of the map. When clicked, the button will:

 * center the map on the user position;
 * follow the user position live on map;
 * rotate the map based on user compass;

To enable the button, use:
```kotlin
channel.invokeMethod("showCenterPositionFab", "true")
```

or, if you have imported our [Flutter Utils](https://github.com/Nextome/nextome-phoenix-android-whitelabel/blob/main/app/src/main/java/com/nextome/test/helper/flutter/FlutterUtils.kt):

```kotlin
  FlutterUtils.setMapViewSettings(channel, 
    fabEnabled = true
  )
```

!!!warning "please note"

    The compass feature will only work if the app has the following permissions:

    * android.permission.INTERNET
    * android.permission.ACCESS_COARSE_LOCATION
    * android.permission.ACCESS_FINE_LOCATION

    If those permissions are not denied, the navigation button will only follow user position without rotating.

## Change user position icon
!!!info
    Available from flutter-map > 1.2.0

The default position icon is a blue dot. If you want to change the icon, you can load a remote resource from an url.

```kotlin
  channel.invokeMethod("customPositionResourceUrl", "http://nextome.net/position-icon.png")
```

or, if you're imported our [Flutter Utils](https://github.com/Nextome/nextome-phoenix-android-whitelabel/blob/main/app/src/main/java/com/nextome/test/helper/flutter/FlutterUtils.kt):

```kotlin
  FlutterUtils.setMapViewSettings(channel,
      customPositionResourceUrl = "http://nextome.net/position-icon.png"
  )
```
