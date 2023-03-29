# Get venue data
You can optionally ask the SDK for the local resources downloaded with your venue (those resources include maps, pois, beacons events...).
The object `NextomeVenueData` contains the complete venue data and some methods to help you perform some queries.

### Venue data while observing state
A `NextomeVenueData` object could be retreived in `getStateObservable when Nextome SDK is in the states `FindFloorState` or `LocalizationRunningState`.

=== "Android"
   ```kotlin
          nextomeSdk.getStateObservable().asLiveData().observe(this) { state ->
             when (state) {
             is LocalizationRunningState -> { 
                 updateState("Showing map of floor ${state.mapId}...")

                 val venueData = state.venueData
                 val currentMap = state.mapId

                 val allMaps = venueData.maps

                 val events = venueData.events
                 val eventsOfMap = venueData.getEventsByMapId(currentMap)

                 val beacons = venueData.beacons
                 val beaconsOfMap = venueData.getEventsByMapId(currentMap)

                 val path = venueData.path
                 val pathNodesOfMap = venueData.getPathNodesOfMapId(currentMap)

                 val settings = venueData.settings
             }
          }
       }
   ```
=== "iOS"
   ```swift
      let watcher = nextomeSdk.getStateObservable().watch(block: {state in
      guard let state = state else {return }
   
                   if let runningState = state as? LocalizationRunningState{
                   
                        let venueData = runningState.venueData
                        let currentMap = runningState.mapId
    
                        let allMaps = venueData.maps
    
                        let events = venueData.events
                        let eventsOfMap = venueData.getEventsByMapId(mapId: currentMap)
    
                        let beacons = venueData.beacons
                        let beaconsOfMap = venueData.getEventsByMapId(mapId: currentMap)
    
                        let path = venueData.path
                        let pathNodesOfMap = venueData.getPathNodesOfMapId(mapId: currentMap)
    
                        let settings = venueData.settings
                       
                   }
               })
   ```

### Query venue data
It is also possible to query venue data while Nextome SDK is not running calling `nextomeSdk.getVenueData(venueId)`.

!!! note "Source of venue data"
    Venue data can be retrieved:
    
    * Offline, from cached data, if the user has been localized or requested venue data recently;
    * Offline, from [initial resources provided](initial-resources.md) to SDK;
    * From Nextome Servers. In this case, data about the venue will be downloaded and cached locally on device;

!!! warning
    If the device hasn't a cached copy of the venue and there is no network connectivity
    available, the method will throw an exception.

=== "Android"
    ```kotlin
    val venueData: NextomeVenueData = nextomeSdk.getVenueData(venueId = myVenueId)
    
    val allMaps = venueData.maps
    
    val events = venueData.events
    val eventsOfMap = venueData.getEventsByMapId(mapId)
    
    val beacons = venueData.beacons
    val beaconsOfMap = venueData.getEventsByMapId(mapId)
    
    val path = venueData.path
    val pathNodesOfMap = venueData.getPathNodesOfMapId(mapId)
    
    val settings = venueData.settings
    ```
=== "iOS"
    ```swift
    nextomeSdk.getVenueData(venueId: myVenueId, completionHandler: {data, error in
        guard error == nil else { return }
        guard let venueData = data else { return }
        
        let allMaps = venueData.maps
        
        let events = venueData.events
        let eventsOfMap = venueData.getEventsByMapId(mapId: mapId)
        
        let beacons = venueData.beacons
        let beaconsOfMap = venueData.getEventsByMapId(mapId: mapId)
        
        let path = venueData.path
        let pathNodesOfMap = venueData.getPathNodesOfMapId(mapId: mapId)
        
        let settings = venueData.settings
    })
    ```
