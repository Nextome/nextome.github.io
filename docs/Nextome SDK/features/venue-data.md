# Get venue data
You can optionally ask the SDK for the local resources downloaded with your venue (those resources include maps, pois, beacons events...).
These resources are wrapped inside a NextomePackage.
This object is available in the NextomeState during FindFloorState and LocalizationRunningState

### Venue data while observing state 
=== "Android"
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
=== "iOS"
   ```swift
      let watcher = nextomeSdk.getStateObservable().watch(block: {state in
      guard let state = state else {return }
   
                   if let runningState = state as? LocalizationRunningState{
                   
                       let poiList = runningState.venuePackage.allPois
                       let poiOfTheCurrentFloor = runningState.venuePackage.getPoisByMapId(mapId: runningState.mapId)
                       let mapsInTheVenue = runningState.venuePackage.maps
                       let beaconsInTheVenue = runningState.venuePackage.beacons
                       let eventsInTheVenue = runningState.venuePackage.events
                       let venueSettings = runningState.venuePackage.settings
                       
                   }
               })
   ```

### Query venue data

=== "Android"
   ```kotlin
      val resources: NextomePackage = nextomeSdk.getVenuePackage(venueId = myVenueId)
   ```
=== "iOS"
   ```swift
      nextomeSdk.getVenuePackage(venueId: myVenueId, completionHandler: {package, error in
         guard error == nil else { return }
         guard let resources = package else { return }
      })
   ```

!!! note
You can only retrieve resources when Nextome SDK State is `RUNNING`. Otherwise, `nextomeSdk.state.valueResources` will return a null object.