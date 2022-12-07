# Android Integration - Getting started

A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel). Run the MapActivity to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.

## Prerequisites
!!!bug
    PLACEHOLDER

- Install or update Android Studio to its latest version
- Make sure that your project meets these requirements: 
    - Target API: 32 or higher
    - Uses Android x or higher
    - Uses Jetpack(Android X)
        - `com.android.tools.build:gradle` v3.2.1 or later
        - `compileSdkVersion` 32 or later
- Have credentials for jFrog
- Have credentials for our portal

## How to include

1. Add our repositories in the Gradle Project Settings `settings.gradle.kts`:

    === "Groovy"
        ``` groovy title="settings.gradle"
        
        dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...
                maven { url "https://jitpack.io" }
                maven {
                    url "https://nextome.jfrog.io/artifactory/nextome-libs-release-local"

                    credentials {
                        username "USERNAME"
                        password "PASSWORD"
                    }
                }
            }
        }
        ```
    === "KTS"
        ``` kotlin title="settings.gradle.kts"
    
        dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...
                maven { url = uri("https://jitpack.io)" }
                maven {
                    url = uri("https://nextome.jfrog.io/artifactory/nextome-libs-release-local")

                    credentials {
                        username = "USERNAME"
                        password = "PASSWORD"
                    }
                }
            }
        }
        ```
    !!! note
        Contact Nextome to receive a valid username/password to access our repository.

2. In your module (app-level) Gradle file, add the dependency for the SDK:

    === "Groovy"

        ``` groovy title="project/build.gradle"
        implementation 'net.nextome.phoenix_sdk:phoenix-sdk:{last_version}'
        ```

    === "KTS"

        ``` kotlin title="project/build.gradle.kts"
        implementation ("net.nextome.phoenix_sdk:phoenix-sdk:{last_version}")
        ```
    Check latest released version [here](/docs/Nextome%20SDK/Android/changelog.md)

## Retrive SDK Credentials
Log-in the web dashboard and retrieve the `Client` and `Secret Key` for the SDK.
Those credentials are available from your profile, in the Apps section. 

![Retrieve SDK Credentials](../../assets/sdk_key.png)

## Required permissions
To run, Nextome SDK requires the following permissions:
```xml title="AndroidManifest.xml"
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

!!!note 
    Those are required to:

    - Contact the backend server;
    - Activate and manage Bluetooth beacon scan;
    - Run as a foreground service for background mode;
    - Access Bluetooth API (location)

## SDK Initialization

To use Nextome SDK for Android initialize the NextomePhoenixSdk.

It requires the `application context`, the given `Client` and `Secret Key`.

```kotlin
    nextomeSdk = NextomePhoenixSdk(
        clientId = CLIENT_ID,
        clientSecret = CLIENT_SECRET,
        context = context as ApplicationContext,
    )
```
!!!note
    By default the SDK works with settings defined in the web portal.
    The NextomePhoenixSdk constructor it is possible to override some of those as described in this section.
    But please notice that this operation is extremely dangerous and should only be made in accordance with the Nextome Team because has an huge impact on the localization's performance.  

## Start localization
To start localization, call:

```kotlin
nextomeSdk.start()
```

If you want Nextome to keep track of user indoor position also while the phone screen is off or your app is in background explore the corresponding section [LINK HERE]

## Stop localization
When you've done, stop the localization by calling:

```kotlin
nextomeSdk.stop()
```

## Observe SDK status
It's possible to observe the current state the Nextome SDK.

You can use this data to start initializing the map or showing messages to the users and update your UI accordingly.

```kotlin
val state: Flow<NextomeSdkState> = nextomeSdk.getStateObservable()
state.asLiveData().observe(this){state -> 
    
}
```
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
| `venueId: Int`  | The venueId of the venue found    |

#### FindFloorState
All the venue resources have been downloaded. Nextome is now computing in which floor the user is;

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `venueId: Int`  | The venueId of the venue found    |
| `venuePackage: NextomePackage` | Contains all the resources(beacons, pois, maps, events, path and settings) for a specific venue. See more [here](additional-features.md)|

#### LocalizationRunningState

Nextome SDK is computing user positions. You can observe live user location using the observer nextomeSdk.locationLiveData;

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `venueId: Int`  | The venueId of the venue found    |
| `venuePackage: NextomePackage`| Contains all the resources(beacons, pois, maps, events, path and settings) for a specific venue. See more [here](additional-features.md)|
| `isOutdoor: Bool`   | Will always be true in this state    |
| `mapId: Int`   | Will always be true in this state    |
| `isOutdoor: Bool`   | Will always be true in this state    |
| `tileZipPath: String`| The path for the zip file which contains the tiles for the current map   |
| `mapHeight: Int`   | The height in pixel of the map   |
| `mapWith: Int`   | The width in pixel of the map    |

#### ErrorState
The SDK was stopped due to a fatal error. It exposes a NextomeException which can be either a GenericException or InvalidCredentialException.

!!!note 
    - If the user **changes floor**, the SDK will resume from `FIND_FLOOR` state.
    - If the user goes **outdoor**, the SDK will switch to `SEARCH_VENUE` state until a new indoor beacon is detected.

### Complete example
??? example "Example: getStateObservable()"
    ```kotlin
    nextomeSdk.getStateObservable().asLiveData().observe(this) { state ->
            when (state) {
                is IdleState -> {
                    showOpenStreetMap()
                    updateState("Sdk is Idle")
                }
                is StartedState -> {
                    showOpenStreetMap()
                    updateState("Sdk Started")
                }

                is SearchVenueState -> {
                    showOpenStreetMap()
                    updateState("Searching Venue...")
                }

                is GetPacketState -> {
                    showOpenStreetMap()
                    updateState("Downloading venue ${state.venueId}...")
                }

                is FindFloorState -> {
                    showOpenStreetMap()
                    updateState("Finding current Floor on venue ${state.venueId}...")
                }

                is LocalizationRunningState -> {
                    updateState("Showing map of floor ${state.mapId}...")
                    showIndoorMap()
                    setIndoorMap(state.tilesZipPath,
                        state.mapHeight,
                        state.mapWidth,
                        state.venuePackage.getPoisByMapId(state.mapId)
                    )
                    poiList = state.venuePackage.allPois
                }

                is ErrorState -> {
                    viewModel.handleError(state.exception)
                }
            }

        }
    ```
## Observe the user position
Nextome SDK offers a flow to observe the user position:

```kotlin
val state: Flow<NextomePosition> = nextomeSdk.getLocalizationObservable()
state.asLiveData().observe(this){position -> 
   log("Got indoor position (${it.x}, ${it.y}), on map ${it.mapId} in floor ${it.floorId})") 
}
```
#### NextomePosition

| Property             | Description                          |
| :--------------------| :----------------------------------- |
| `x: Double`   | The x coordinates of the localized point.|
| `y: Double`  | The y coordinates of the localized point. |
| `venueId: Int`| The venueId of the venue found. |
| `label: String?`   | A label associated with the position.   |

## Observe errors
!!!bug
    PLACEHOLDER

TO BE IMPLEMENTED

<!-- 
## Debug tools
In case of necessity, Nextome SDK can write logs with useful info to a file.
You can start writing logs with `nextomeSdk.startLoggingOnFile()`. Those logs can then be shared using `nextomeSdk.stopAndShareLog(context)`. A new Intent will be fired with a share dialog that allows to export and save the logs with other apps.
-->

## Offline capability
With the migration from 0.X.X to 1.X.X the SDK can also work offline by default if the resources of the venue were downloaded at least the first time. 
It is also possible to provide the SDK initial resources to let the app work offline from the start. More info are available [here](additional-features.md).

## Next steps

- Visit [Background-service](background-service.md) to learn how to use the SDK even when the app is not opened in the foreground.

- See the [Additional features](additional-features.md) available, like events observers.

- Visit [Flutter map integration](flutter-map-integration.md)if you want to use our library to display the indoor map.

## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.


<br>

**© 2021 Nextome srl | All Rights Reserved.**