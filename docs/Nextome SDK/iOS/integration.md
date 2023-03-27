# iOS Integration - Getting started

A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel). Run to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.

## Prerequisites

- Xcode 14.2
- Make sure that your project meets these requirements: 
    - Swift 5.7
    - Minimum deployment: iOS 13.2
- Have credentials for jFrog
- Have credentials for our portal

## How to include

### Cocoapods

Phoenix Sdk is distributed with [Cocoapods](https://guides.cocoapods.org/) and artifactory. Be sure to have CocoaPods installed, or follow [this guide](https://guides.cocoapods.org/using/getting-started.html) to install it.

Then it is necessary to configure our private Spec Repo.

1. Install the cocoapods-art plugin
    ``` bash 
    gem install cocoapods-art
    ```

    !!! note
        It is recommended to install both the CocoaPods client and the cocoapod-art plugin as gems

2. Add artifactory credentials  in the netrc file

    ``` bash 
    nano ~/.netrc
    ```
    Copy this then close and save

    ```
    machine nextome.jfrog.io
    login <USERNAME>
    password <ENCRYPTED-PASSWORD>
    ```


    !!! note
        If you get an error of this type: "Permission bits, should be 0600, but are 644"
        
        Run this command: 

        `chmod 0600 ~/.netrc`
     

3. Add an Artifactort repository
    ``` bash 
    pod repo-art add nextome-cocoapods-local "https://nextome.jfrog.io/artifactory/api/pods/nextome-cocoapods-local"
    ```

4. Synchronize the cocoapods-art plugin with artifactory.

    As opposed to the cocoapods client's default behavior, the cocoapods-art plugin does not automatically update its index whenever you run client commands (such as install). To keep your plugin's index synchronized with your CocoaPods repository, you need to update it by executing the following command:

    ```bash
    pod repo-art update
    ```

5. Create a Podfile if you don't already have one. From the root of your    project directory, run the following command

    ```bash
    pod repo-art update
    ```

6. To your Podfile, be sure that the platform is at least 13.2 then ad the CocoaPods specs source and the cocoapods-art plugin. Then add the PhoenixSdk pod

    ```
    platform :ios, '13.2'

    source 'https://github.com/CocoaPods/Specs.git'

    plugin 'cocoapods-art', :sources => [
        'nextome-cocoapods-local'
    ]

    pod 'PhoenixSdk'
    ```

7. Install the pods, then open your .xcworkspace file to see the project in Xcode

    ```bash
    pod install
    ```

## Setup

In order to work properly the SDK requires to setup some permissions and capabilities:

### Add Background capabilities

1. Select your project in Xcode’s Project navigator.
2. Select the app’s target in the Targets list.
3. Click the Signing & Capabilities tab in the project editor.
4. Add the background capability

    ![Add Capability](../../assets/addCapabilities.png)

5. Add background processing and Location Updates

    ![Background capability](../../assets/backroundCapability.png)

 
### Add permissions

Add these permissions in the `info.plist`:

1. Privacy - Location Always and When In Use Usage Description
2. Privacy - Location Always Usage Description
3. Privacy - Location When In Use Usage Description

### Retrive SDK Credentials
Log-in the web dashboard and retrieve the `Client` and `Secret Key` for the SDK.
Those credentials are available from your profile, in the Apps section. 

![Retrieve SDK Credentials](../../assets/sdk_key.png)

## SDK Initialization

To use Nextome SDK for iOS initialize the NextomePhoenixSdk.

It requires the given `Client` and `Secret Key`.

```swift
    let nextomeSdk = NextomePhoenixSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET).build()
```

!!!note
    By default the SDK works with settings defined in the web portal.
    The NextomePhoenixSdk.Builder allows to override some of those as described in the next sections.
    But please notice that this operation is extremely dangerous and should only be made in accordance with the Nextome Team because has an huge impact on the localization's performance.  

## Start localization
To start localization, call:

```kotlin
nextomeSdk.start()
```

## Stop localization
When you've done, stop the localization by calling:

```kotlin
nextomeSdk.stop()
```

## Observe SDK status
It's possible to observe the current state of the Nextome SDK.

You can use this data to start initializing the map or showing messages to the users and update your UI accordingly.

```swift
let observer = nextomeSdk.getStateObservable().watch {state in
            
}
```
!!!warning
    Be sure to [detach the observer](#detach-the-observer)  when you don't need it anymore. 

### Nextome SDK State
`NextomeSdkState` is a simple state machine that can have different states:
<figure markdown>
  ![State Flows](../../assets/flussoSDK.jpeg)
  <figcaption>Nextome SDK State</figcaption>
</figure>

#### IdleState
Nextome SDK has been initialized but there is no active localization service running.

#### StartedState
Nextome has been correctly initialized and started, it's ready to scan beacons;

| Property           | Description                          |
| :------------------| :----------------------------------- |
| `isOutdoor: Bool`  | Will always be true in this state    |

#### SearchVenueState
Nextome is currently scanning nearby beacons to determine in which venue the user is; If the SDK is stuck here, you're probably outdoor.

| Property           | Description                          |
| :------------------| :----------------------------------- |
| `isOutdoor: Bool`  | Will always be true in this state    |

#### GetPacketState
Nextome knows the venue of the user and it's downloading from the server the associated resources (Maps, POIs, Patches...);

| Property           | Description                          |
| :------------------| :----------------------------------- |
| `isOutdoor: Bool`  | Will always be true in this state    |
| `venueId: Int32`  | The venueId of the venue found    |

#### FindFloorState
All the venue resources have been downloaded. Nextome is now computing in which floor the user is;

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `venueId: Int32`  | The venueId of the venue found    |
| `venuePackage: NextomePackage` | Contains all the resources(beacons, pois, maps, events, path and settings) for a specific venue. See more [here](additional-features.md)|

#### LocalizationRunningState

Nextome SDK is computing user positions. You can observe live user location using the observer nextomeSdk.locationLiveData;

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `venueId: Int32`  | The venueId of the venue found    |
| `venuePackage: NextomePackage`| Contains all the resources(beacons, pois, maps, events, path and settings) for a specific venue. See more [here](additional-features.md)|
| `isOutdoor: Bool`   | Will always be true in this state    |
| `mapId: Int32`   | Will always be true in this state    |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `tileZipPath: String`| The path for the zip file which contains the tiles for the current map   |
| `mapHeight: Int32`   | The height in pixel of the map   |
| `mapWith: Int32`   | The width in pixel of the map    |

!!!note 
    - If the user **changes floor**, the SDK will resume from `FIND_FLOOR` state.
    - If the user goes **outdoor**, the SDK will switch to `SEARCH_VENUE` state until a new indoor beacon is detected.

### Complete example
??? example "Example: getStateObservable()"
    ```swift
    let watcher = nextomeSdk.getStateObservable().watch(){state in

            guard let state = state else {return }

            if state is IdleState{

                self.showOpenStreetMap()
                self.updateState(value: "Sdk is in Idle")

            }else if state is StartedState{

                self.showOpenStreetMap()
                self.updateState(value: "Sdk is Started")

            }else if state is SearchVenueState{

                self.showOpenStreetMap()
                self.updateState(value: "Sdk is searching for a venue")

            }else if let getPacketState = state as? GetPacketState{

                self.showOpenStreetMap()
                self.updateState(value: "Downloading venue \(getPacketState.venueId)...")

            }else if let findFloorState = state as? FindFloorState{

                self.showOpenStreetMap()
                self.updateState(value: "Finding current Floor on venue \(findFloorState.venueId)...")

            }else if let runningState = state as? LocalizationRunningState{

                self.showIndoorMap()

                var mapPois = runningState.venuePackage.getPoisByMapId(mapId: runningState.mapId)
                self.setIndoorMap(runningState.tilesZipPath,
                                  runningState.mapHeight,
                                  runningState.mapWidth,
                                  mapPois)

                var allPois = runningState.venuePackage.allPois

            }

        }
    ```

## Observe the user position
Nextome SDK offers an observer on the user position:

```swift
let observer = nextomeSdk.getLocalizationObservable().watch() {position in
            guard let position = position else {return}
            print("Got indoor position \(position.label ?? "-") (\(position.x), \(position.y), on map \(position.mapId) in venue \(position.venueId)")
}
```
#### NextomePosition

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `x: Double`   | The x coordinates of the localized point.|
| `y: Double`  | The y coordinates of the localized point. |
| `mapId: Int32`| The mapId of the position. |
| `venueId: Int32`| The venueId of the venue found. |
| `label: String?`   | A label associated with the position.   |

!!!warning
    Be sure to [detach the observer](#detach-the-observer)  when you don't need it anymore. 

## Detach the observer
When you are no longer interested in observing the events, you must detach your listener so that your event callbacks stop getting called.

```swift
observer.close()
```

## Offline capability
With the migration from 0.X.X to 1.X.X the SDK can also work offline by default if the resources of the venue were downloaded at least the first time. 
It is also possible to provide the SDK initial resources to let the app work offline from the start. More info are available [here](additional-features.md).

## Next steps

- See the [Additional features](additional-features.md) available, like events observers.

- Visit [Flutter map integration](flutter-map-integration.md)if you want to use our library to display the indoor map.

## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.


<br>

**© 2021 Nextome srl | All Rights Reserved.**