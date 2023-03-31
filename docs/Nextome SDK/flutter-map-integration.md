---
title: Nextome map integration
hide:
  - footer
---
# Nextome map integration
If you want, there’s an optional module that can show a live map of the indoor location of the user. 
You havo two options to integrate the UI component:

1. Integrate the `NextomeMapUtils` (Recommended method), which is a wrapper that allows you to integrate easly the SDK with the Map module
2. Integrate directly the `NextomeFlutterMap`. This method requires more bolerplate code but allows more customization

<p style="text-align: center;"><img src="/assets/mapIos.jpeg" width="50%"></p>

## Integrate NextomeMapUtils
In this sections we will go through the integration of the NextomeMapUtils. If you're interested in th NextomeFlutterMap approach you can refer to this documentation. <INSERISCI LINK A FLUTER MAP>

### Install the dependency

=== "Android"
    //TODO @Paolo

=== "iOS"
    1. Add the pod dependency inside the `Podfile`
        ```swift
        pod NextomeMapUtils_Release
        ```

        !!! note
            The NextomeMapUtils is distributed in two different pod version, `Release` and `Debug`. Th release version will compile also for the simulate but will not show the map view. To test the map on the simulator you can use the `Debug` version instead. 
            It is important to use the `Release` version for the app store. 

    2. Install the dependency
        ```swift
        pod install
        ```

### Initialization 

The first step is to initialize the module.

=== "Android"
    //TODO @Paolo

=== "iOS"

    1. Open th `AppDelegate`
    2. Import the module
        ```swift
        import NextomeMapUtils
        ```
    3. Initialize the `NextomeMapHandler` in the `didFinishLaunchingWithOptions` method:
        ```swift
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
            // Override point for customization after application launch.
            NextomeMapHandler.instance.initialize()
            return true
        } 
        ```
    
### Integration

The `NextomeMapUtils` provide a UIViewController/Fragment and some methods to handle it.

1. Import the module

    === "Android"
        //TODO @Paolo

    === "iOS"
        
        ```swift
        import NextomeMapUtils
        ```

2. Initialize the Fragment/UIViewController

    === "Android"
        //TODO @Paolo

    === "iOS"
        The `NextomeMapHandler` provides a UIViewController that you can use to show the flutter map.
        ```swift
        lazy var indoorMapViewController: UIViewController = NextomeMapHandler.instance.initializeFlutterViewController()
        ```

### Show a new indoor map

Once a map is available, pass the `tiles local directory`, `height` and `width` of the map to the `NextomeMapHandler`.
Those parameters can be obtained from the `LocalizationRunningState` during localization or from the specific map returned in the `NextomeVenueData`. More info on the venue data can be retrieve [here](features/venue-data.md).
=== "Android"
    //TODO @Paolo

=== "iOS"
    ```swift
    NextomeMapHandler.instance.setMap(tilesZipPath: runningState.tilesZipPath,
                                        mapHeight: runningState.mapHeight,
                                        mapWidth: runningState.mapWidth)
    ```

### Update user live position

When a new indoor user position is available, notify `NextomeMapHandler` to update the blue point with:

=== "Android"
        //TODO @Paolo

=== "iOS"
    ```swift
    func observeSdkLocations(){
    let watcher = nextomeSdk.getLocalizationObservable().watch(block: {position in
        if let position = position{
            self.lastPosition = position
            NextomeMapHandler.instance.updatePositionOnMap(position)
        }
    })

    watchers.append(watcher)
    }   
    ```

### Show a list of Point of Interest
You can set the list of Point of Interest to show on the map using this method:
   
=== "Android"
    //TODO @Paolo

=== "iOS"
    ```swift
    let pois: [NextomePoi] = runningState.venueData.getPoisByMapId(mapId: runningState.mapId)
    NextomeMapHandler.instance.updatePoiList(pois)
    ```

### Show a path between two points on map
You can show a path on the map using this method and passing a list of Vertex. You can automatically get the Vertext between a start and end point of the map using the appropriate method on the [nextomeSdk](features/navigation.md):

=== "Android"
    //TODO @Paolo

=== "iOS"
    ```swift
    func showPath(path: [Vertex]){
        NextomeMapHandler.instance.updatePath(path: path)
    }
    ```

### Observe events on Flutter Map
With this observer you can be notified of events on the map, for example, when the user decides to navigate to a POI.

=== "Android"
    //TODO @Paolo

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        func onPoiClicked(poi: NextomePoi){

        }
        ```

### Show Center Position Fab
It’s possible to show an optional fab at the bottom right of the map. When clicked, the button will:

- center the map on the user position;
- follow the user position live on map;
- rotate the map based on user compass;

To enable the button, use:

=== "Android"
    //TODO @Paolo

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        NextomeMapHandler.instance.setMapSettings(fabEnabled: true)
        ```

### Change user position icon
The default position icon is a blue dot. If you want to change the icon, you can load a remote resource from an url.
=== "Android"
    //TODO @Paolo

    !!! note
        The compass feature will only work if the app has the following permissions:
        ``` 
        android.permission.INTERNET
        android.permission.ACCESS_COARSE_LOCATION
        android.permission.ACCESS_FINE_LOCATION
        ```
        If those permissions are not denied, the navigation button will only follow user position without rotating.

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        let remotePath = ""http://nextome.com/position-icon.png""
        NextomeMapHandler.instance.setMapSettings(customPositionResourceUrl: remotePath)
        ```