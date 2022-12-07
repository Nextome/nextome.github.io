
# Additional Features
TODO
## Get venue resources
You can optionally ask the SDK for the local resources downloaded with your venue (those resources include maps, pois, beacons events...).
There are two ways of doing this.

In the state observer:
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

as a single method call:
```kotlin
    val resources = nextomeSdk.state.venueResources
```
!!! note
    You can only retreive resources when Nextome SDK State is `RUNNING`. Otherwhise, `nextomeSdk.state.valueResources` will return a null object.


!!!example
    ```kotlin
    nextomeSdk.stateLiveData.observe(this, {
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
                    setIndoorMap(mapTilesUrl, mapHeight, mapWidth, venueResources.getPoisByMapId(mapId))
                }
            }
        }
    })
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
!!! example
    ```kotiln
    // Force map 42
    nextomeSdk.setForcedMap(42)

    nextomeSdk.stateLiveData.observe(this, {
        when (it.state) {
            NextomePhoenixState.RUNNING -> {
                with(it) {
                    // After forcing the map, here you can pick tiles, height and width of map 42.
                    setIndoorMap(mapTilesUrl, mapHeight, mapWidth, venueResources.getPoisByMapId(mapId))
                }
            }
        }
    })
    ```

### Calcualte path
Nextome Sdk is able to calculate the shortest path between two points of the same venue.

The function takes (x, y) coordinates and map of the starting point and (x, y) coordinates and map of the destination point.

```kotlin
val path = nextomeSdk.findPath(
                startX.toInt(), startY.toInt(), startMapId,
                targetX.toInt(), targetY.toInt(), targetMapId)
```

The returned path is a list of ordered Vertex, that, for example, can be passed to the Flutter Map Module and displayed to the user (see the methods written below).

### Observe events
!!!info 
    Available from nexome-sdk > 0.4.0

If events are set up in your venue, you can observe when user enters or exits from event radius using those two observers:
```kotlin
    nextomeSdk.enterEventObservable.observe(this) { event ->
        Log.d("event_test", "Received on enter with data: ${event.data}")
    }

    nextomeSdk.exitEventObservable.observe(this) { event ->
        Log.d("event_test", "Received on exit with data: ${event.data}")
    }
```


Optionally, after exiting from event, it's possible to set a timeout value in seconds before signaling that event again.

This allows you to ignore when user exits and enters again in a event radius in a short time span. For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute.

If set to 0, Nextome SDK will signal onEnter and onExit event realtime.

```kotlin
    nextomeSdk = NextomePhoenixSdk().Builder(applicationContext)
        .withEventTimeoutDurationInSeconds(10L)
```