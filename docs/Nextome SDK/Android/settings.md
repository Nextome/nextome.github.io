# Override default settings
TODO
Durning the NextomePhoenixSdk initialization it is possible to override some of the settings defined in the web portal.
These changes will be overridden for all the venues.
It is not recommended to change these parameters directly from the app, use the Venue Settings available from the web portal instead. 

!!! danger

    Overriding these parameters is an extremely dangerous operation that should only be made in accordance with the Nextome Team because has an huge impact on the localization's performance.  


#### Scanner parameters
!!! note
    The longer you wait between scans, the longer it will take to detect a beacon. And the more reduce the length of the scan, the more likely it is that you might miss an advertisement from an beacon.


You can optionally customize scan period and scan duration:

```kotlin
scanPeriod: Long
```

sets a time in millis for the bluetooth beacon scan duration.
> Default value if not customized in the web portal is `1000 ms`

___

```kotlin
betweenScanPeriod: Long`
```

sets a time in millis to wait between bluetooth scans.

> Default value if not customized in the web portal is `250 ms`.

___

#### Other optional parameters
```kotlin
rssiThreshold: Int
```

ignore beacons below a custom RSSI threshold.
> Default value if not customized in the web portal is `-75`.

___

```kotlin
beaconListMaxSize: Int`
```

keeps in memory a cache of `n` RSSI entries in time for each beacon to more accurately compute user position.

> Default value if not customized in the web portal is `12`

___

```kotlin
localizationMethod: NextomeLocalizationMethod`
```

Enum that sets the algorithm for localization. Choices are `LINEAR_SVD, NON_LINEAR_DA, PARTICLE`.

> Default value if not customized in the web portal is `LINEAR_SVD`
___

```kotlin
particleActive: Boolean`
```

Boolean that indicates if the particle filter is enabled or not.

> Default value if not customized in the web portal is `true`


___

```kotlin
forceVenueId: Int
```

This allows you to skip the search for venue phase and automatically download resurces of a specific venue when Nextome starts.

> With this, you can force your app to open only your venue and download venue resurces even if you're not in range of your venue beacons.

___

```kotlin
sendPositionToServer: Boolean
```

This allows you to automatically send positions real time to Nextome server and track monitor devices realtime on the web manager.

>Default value if not customized in the web portal is `false`

___

```kotlin
.eventTimeoutDurationInSeconds: Int
```

This allows you to ignore when user exits and enters again in a event radius in a short time span. For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute.

>Default value if not customized in the web portal is `0 seconds` (realtime).

___