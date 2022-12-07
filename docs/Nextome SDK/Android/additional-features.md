# Additional Features
Besides from the user position live update, other features are available with the Nextome SDK:

- Observe events;
- Get venue resources;
- Initial resources;
- Force a specific venue;
- Pathfinder;

## Observe events

If events are set up in your venue, you can observe when user enters or exits from event radius using those two observers:
```kotlin
nextomeSdk.getEnterEventObservable().asLiveData().observe(this) { event ->
    Log.e("event_test", "Received on enter with id ${event.event.id} and data: ${event.event.data}")            
}

nextomeSdk.getExitEventObservable().asLiveData().observe(this) { event ->
    Log.e("event_test", "Received on exit with id ${event.event.id} and data: ${event.event.data}")
}
```


Optionally, after exiting from event, it's possible to set a timeout value in seconds before signaling that event again.
This allows you to ignore when user exits and enters again in a event radius in a short time span. For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute.
If set to 0, Nextome SDK will signal onEnter and onExit event realtime.
This parameter is only configurable during the SDK initialization.

``` kotlin
nextomeSdk = NextomePhoenixSdk(
    clientId = CLIENT_ID,
    clientSecret = CLIENT_SECRET,
    context = context as ApplicationContext,
    eventTimeoutDurationInSeconds = 10
)
```

## Get venue resources
You can optionally ask the SDK for the local resources downloaded with your venue (those resources include maps, pois, beacons events...).
These resources are wrapped inside a NextomePackage.
This object is available in the NextomeState during FindFloorState and LocalizationRunningState 

1. In the state observer:
    ```kotlin
    nextomeSdk.getStateObservable().asLiveData().observe(this) { state ->
            when (state) {
                is LocalizationRunningState -> {
                    updateState("Showing map of floor ${state.mapId}...")
                    val poiList = state.venuePackage.allPois
                    val poiOfTheCurrentFloor = state.venuePackage.getPoisByMapId(state.mapId)
                    val mapsInTheVenue = state.venuePackage.maps
                    val beaconsInTheVenue = state.venuePackage.beacons
                    val eventsInTheVenue = state.venuePackage.events
                    val venueSettings = state.venuePackage.settings
                }
            }

        }
    ```

2. As a single suspend method call:
    ```kotlin
    val resources: NextomePackage = nextomeSdk.getVenuePackage(venueId = myVenueId)
    ```

    !!! note
        You can only retrieve resources when Nextome SDK State is `RUNNING`. Otherwise, `nextomeSdk.state.valueResources` will return a null object.

## Initial Resources
With the migration from v0 to v1 the SDK can also work offline by default but only if the resources of the venue were downloaded at least the first time. 
It is also possible to provide the SDK initial resources to let the app work offline from the start.

The zip file with the initial resources can be downloaded from the web portal and placed inside the assets folder of the app.
Than it can be passed to the SDK, during the initialization, specifying the zip name and, eventually, a version number. 
Please, update the version number incrementally when updating the zip file otherwise the update will be ignored by the SDK. 

``` kotlin
nextomeSdk = NextomePhoenixSdk(
    clientId = CLIENT_ID,
    clientSecret = CLIENT_SECRET,
    context = context as ApplicationContext,
    initialData = AssetResource(name = "name.zip", version = 1)
)
```
If you don't want to bundle the initial data inside the assets folder (for example to download it from an API) you can pass the local path by using the `LocalResource` instead of `AssetResource`.

``` kotlin
nextomeSdk = NextomePhoenixSdk(
    clientId = CLIENT_ID,
    clientSecret = CLIENT_SECRET,
    context = context as ApplicationContext,
    initialData = LocalResource(path = LOCAL_PATH, version = 1)
)
```

## Force a specific venue
It is possible to constraint the localization only to a predetermined venue.
During the SDK Initialization, specify the `forcedVenueId`, this way the SDK will remain in `SearchVenueState` until it localize the phone inside the venue. Other venues will be ignored and if detected the SDK will return to `SearchVenueState`
```kotlin
nextomeSdk = NextomePhoenixSdk(
    clientId = CLIENT_ID,
    clientSecret = CLIENT_SECRET,
    context = context as ApplicationContext,
    forcedVenueId = FORCED_VENUE_ID
)
```
!!! note
    If you want to always show a specific indoor map and enable the localization only for a specific venue you can combine `Initial resources` and `Force specific venue` functionality.
    Use `getVenueResources` to retrieve the image resources to  initialize the map and use the callback from the `Getting started` section to react on position updates and floor changes.
    
## Pathfinder
Nextome Sdk is able to calculate the shortest path between two points of the same venue.

The function takes (x, y) coordinates and map of the starting point and (x, y) coordinates and map of the destination point.

```kotlin
val path = nextomeSdk.findPath(
                startX.toInt(), startY.toInt(), startMapId,
                targetX.toInt(), targetY.toInt(), targetMapId)
```

The returned path is a list of ordered Vertex, that, for example, can be passed to the Flutter Map Module and displayed to the user (see the methods written below).

