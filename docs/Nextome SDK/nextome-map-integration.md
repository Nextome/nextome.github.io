---
title: Nextome map integration
hide:
  - footer
---
# Nextome map integration
If you want, there’s an optional module that can show a live map of the indoor location of the user. 
You have two options to integrate the UI component:

1. Integrate the `PhoenixMapUtils` (Recommended method), which is a wrapper that allows you to integrate easily the SDK with the Map module
2. Integrate directly the `NextomeMap`. This method requires more boilerplate code but allows more customization

<p style="text-align: center;"><img src="/assets/mapIos.jpeg" width="50%"></p>

## Integrate PhoenixMapUtils
:octicons-tag-24: PhoenixMapUtils 1.4.3

In this sections we will go through the integration of the PhoenixMapUtils. If you're interested in th NextomeMap approach you can refer to this documentation.

### Install the dependency

=== "Android"
    1. In your `build.gradle` file, add dependencies for Nextome MapView and PhoenixMapUtils:
    ```kotlin
        implementation("com.nextome.phoenix_map_utils:phoenix_map_utils:1.4.3")
        implementation("net.nextome.nextome_map_module:flutter:1.4.3")
    ```

=== "iOS"
    1. Add the Artifactory repository
    ```swift
    pod repo-art add nextome-cocoapods-local "https://nextome.jfrog.io/artifactory/api/pods/nextome-map-cocoapods-local"
    ```
    2. Update the `Podfile` adding the source, the pod dependency and the post install script. You don't need to specify the Phoenix sdk because it is already defined as PhoenixMapUtils dependency.
    ```swift
    platform :ios, '13.2'

    source 'https://github.com/CocoaPods/Specs.git'

    plugin 'cocoapods-art', :sources => [
        'nextome-map-cocoapods-local',
        'nextome-sdk-cocoapods-local'
    ]

    use_frameworks!

    target 'MyApp' do
        pod 'PhoenixMapUtils_Release'
    end

    post_install do |installer|
        installer.generated_projects.each do |project|
            project.targets.each do |target|
                target.build_configurations.each do |config|
                    config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.2'
                end
            end
        end
    end
    ```

        !!! note
            The PhoenixMapUtils is distributed in two different pod version, `Release` and `Debug`. Th release version will compile also for the simulate but will not show the map view. To test the map on the simulator you can use the `Debug` version instead. 
            It is important to use the `Release` version for the app store. 

    3. Install the dependency
        ```swift
        pod install
        ```

### Initialization 

The first step is to initialize the module.

=== "Android"
    1. In your .xml layout, add a `PhoenixMapView` where you want to display the indoor map.
    ```xml
        <com.nextome.phoenix_map_utils.PhoenixMapView
            android:id="@+id/indoor_map"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    ```
    2. In your Activity/ViewModel, initialize `PhoenixMapHandler`
    ```kotlin
        val handler = PhoenixMapHandler()

        val fragmentManager: FragmentManager = supportFragmentManager
        handler.initialize(
            fragmentManager = fragmentManager,
            viewId = R.id.indoor_map, // View created before
            context = this
        )
    ```

=== "iOS"
    1. Open th `AppDelegate`
    2. Import the module
        ```swift
            import PhoenixMapUtils
        ```
    3. Initialize the `PhoenixMapHandler` in the `didFinishLaunchingWithOptions` method:
        ```swift
            func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
                // Override point for customization after application launch.
                PhoenixMapHandler.instance.initialize()
                return true
            } 
        ```
    The `PhoenixMapHandler` provide a UIViewController/Fragment and some methods to handle it.
    ### Integration
    1. Import the module
        ```swift
            import PhoenixMapUtils
        ```
    2. Initialize the Fragment/UIViewController
    The `PhoenixMapHandler` provides a UIViewController that you can use to show the flutter map.

    ```swift
        lazy var indoorMapViewController: UIViewController = PhoenixMapHandler.instance.initializeFlutterViewController()
    ```

### Show a new indoor map

Once a map is available, pass the `tiles local directory`, `height` and `width` of the map to the `PhoenixMapHandler`.
Those parameters can be obtained from the `LocalizationRunningState` during localization or from the specific map returned in the `NextomeVenueData`. More info on the venue data can be retrieve [here](features/venue-data.md).
=== "Android"
    `handler.setMap(mapTilesUrl = mapTilesUrl, mapHeight = mapHeight, mapWidth = mapWidth)`

=== "iOS"
    ```swift
    PhoenixMapHandler.instance.setMap(tilesZipPath: runningState.tilesZipPath,
                                        mapHeight: runningState.mapHeight,
                                        mapWidth: runningState.mapWidth)
    ```

### Update user live position

When a new indoor user position is available, notify `PhoenixMapHandler` to update the blue point with:

=== "Android"
    ```kotlin
        viewModel.nextomeSdk.getLocalizationObservable().asLiveData().observe(this) {
            handler.updatePositionOnMap(it)
        }    
    ```

=== "iOS"
    ```swift
        func observeSdkLocations(){
        let watcher = nextomeSdk.getLocalizationObservable().watch(block: {position in
            if let position = position{
                self.lastPosition = position
                PhoenixMapHandler.instance.updatePositionOnMap(position)
            }
        })
    
        watchers.append(watcher)
        }   
    ```

### Show a list of Point of Interest
You can set the list of Point of Interest to show on the map using this method:
   
=== "Android"
    ```kotlin
    handler.updatePoiList(pois)
    ```

=== "iOS"
    ```swift
    let pois: [NextomePoi] = runningState.venueData.getPoisByMapId(mapId: runningState.mapId)
    PhoenixMapHandler.instance.updatePoiList(pois)
    ```

### Show a path between two points on map
You can show a path on the map using this method and passing a list of Vertex. You can automatically get the Vertext between a start and end point of the map using the appropriate method on the [nextomeSdk](features/navigation.md):

=== "Android"
    ```kotlin
    hanlder.updatePath(path)
    ```

=== "iOS"
    ```swift
    func showPath(path: [Vertex]){
        PhoenixMapHandler.instance.updatePath(path: path)
    }
    ```

### Observe events on Flutter Map
With this observer you can be notified of events on the map, for example, when the user decides to navigate to a POI.

=== "Android"
    ```kotlin 
    handler.observeEvents().collect { event ->
        when (event) { 
            is PhoenixMapHandler.OnNavigationSelected -> {
                // user clicked on "Navigate to POI"
                val poi = event.poi
            } 
            is PhoenixMapHandler.OnPoiClicked -> { 
                // user clicked on poi 
                val poi = event.poi 
            }
        } 
    }
    ```

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user clicked on a POI.

        ```swift
        func onPoiClicked(poi: NextomePoi){

        }
        ```
    3. Implement the onNavigationSelected method to be notified when the user wants to navigate to a POI.

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
    `handler.setMapViewSettings(fabEnabled = true)`

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        PhoenixMapHandler.instance.setMapSettings(fabEnabled: true)
        ```

### Change user position icon
The default position icon is a blue dot. If you want to change the icon, you can load a remote resource from an url.
=== "Android"
    `handler.setMapViewSettings(customPositionResourceUrl = "url")`
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
        PhoenixMapHandler.instance.setMapSettings(customPositionResourceUrl: remotePath)
        ```