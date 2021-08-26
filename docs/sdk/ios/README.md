# PhoenixSdk

PhoenixSdk is an indoor positioning, mobile-ready, offline, Sdk, designed to be integrated into an existing mobile application effortlessly

 - Ready to use
 - Modular Features
 - iOS Optimized

# Features

 - **Localize** user into indoor/outdoor area thanks to low-signal Bluetooth beacons and GPS
 - **Calculate** user realtime position
 - **Monitoring** indoor/outdoor switching and change floor
 - **Elaborate** custom road to point of interest

You can also:
 - **Integrate** this module into an existing app to customize UX
 - **Manipulate** indoor/outdoor user's position coordinate freely

## Requirements

* Swift 5
* iOS 13.2
* xcode 12

## Installation

### Cocoapods

[CocoaPods](https://guides.cocoapods.org/) is a dependency manager for Cocoa projects. For usage and installation instructions, visit their website. 
To integrate PhoenixSdk into your Xcode project using CocoaPods, open your terminal:

```sh
pod repo add NextomeSdk https://gitlab.com/Nextome/nextome-ios-sdk
```

Open your Podfile and add these lines:

After add this pod: 
```pod
  pod 'PhoenixSdk', :git => 'https://github.com/Nextome/POD-Nextome-Sdk'
  pod 'NextomeLegacy', :git => 'https://github.com/Nextome/POD-Nextome-Sdk'
```

Then, into general tab, add this capabilities:
- ***Background Modes***
 - Uses Bluetooth LE accessories
 - Location Updates

Finally, add this line to your Info.plist
- Privacy - Location Always and When In Use Usage Description
- Privacy - Location Always Usage Description
- Privacy - Location When In Use Usage Description

**DONE**

## ONLY FOR MAP TEST PURPOSE
If you want only to test map, you can use x86 framework version.
Remove previous integration lines and replace with these, into your podfile:

```pod
  pod 'PhoenixSdk_x86', :git => 'https://github.com/Nextome/POD-Nextome-Sdk'
  pod 'NextomeLegacy_x86', :git => 'https://github.com/Nextome/POD-Nextome-Sdk'
```

##Notice
1. Into iOS simulator, init Nextome Sdk with override mode; specifying venue and map (you will read more details as you go along), because Simulator doesn't support Bluetooth.

2. Replace x86 pod with arm64 before send to store, because Apple rejects all type of project which contains third party library that integrates simulator architecture.

### How it works

PhoenixSdk is currently design with public async observer

| Info | Observer |
| ------ | ------ |
| User position | POSITION_STREAM |
| Floor change | FLOOR_CHANGE |
| Indoor/outdoor switch | OUTDOOR_STREAM |
| General Log | LOG_WRITER |
| Error | ERROR |

### Getting Started

1. Create Nextome.plist file and insert it, into your main's app Target -> (You can see here: [Nextome.plist](https://github.com/Nextome/POD-Nextome-Sdk/blob/master/README/Nextome.plist))
Insert bundle data and you can adjust settings* for sdk.
**Settings Detail**

 | Setting | Detail |
 | ------ | ------ |
 | Particle Engine | Correction of user position trajectory |
 | Position Log | Send last calculate user position to Nextome server, to improve sdk |
 | Mode | "BLE" localize user venue through beacon monitoring.
 | | "GPS" localize user venue through gps |
 | Binaries | -Not implemented yet- |

## Notice
You can modify these settings, easily calling NextomeSdk, public interface:
- sdkMode = "GPS" or "BLE"
- enableParticleEngine = true/false
- enableSendLog = true/false
- enableBinaries = true/false

2. Import and initialize Sdk, into your ***AppDelegate***
 ```swift
 import PhoenixSdk
 ```
 ```swift
 var sdk: NextomeSdk?
 ```

 After didFinishLaunchingWithOptions
 ```swift
 self.sdk = NextomeSdk(device: String? = nil, logTimer: Int? = nil, venueId: Int? = nil, floorId: Int? = nil)
 ```
 
 | Optional override parameter | Detail |
 | ------ | ------ |
 | device | Set custom device data |
 | logTimer | Customize send log to Nextome server timer |
 | venueId | Ovveride venue id and skipping localization |
 | floorId | Ovveride current map   |


 ## Notice
 If you specificy VenueId and FloorId, Sdk localization's feature will turn off.
 NextomeSdk will only download maps and display theme.

 Otherwise, if you want to init NextomeSdk with localization enabled; without customizations, you can easily do this:
 ```swift
 self.sdk = NextomeSdk()
 ```

***DONE***

## Bonus 1: User Position
If you need to show, user position on virtual map, follow this step to integrate compiled flutter framework into your project: 

1) open xworkspace and import our compiled flutter module, which you can find [here](https://github.com/Nextome/POD-Nextome-Sdk/releases/tag/1.2.0)
2) Download and open flutter_map.zip
3) Choose configuration: Debug, Release, Profile
4) drag & drop frameworks, into main workspace
5) check "Copy items if needed" into alert windows
6) click "general" tab of workspace -> "Frameworks, Library section"
7) set "Embed & Sign in" of whole of them, except for FlutterPluginRegistant

## Bonus 2: Customize flutter map
By default, Sdk use *Compiled* Nextome Flutter Map cross-plattform module, developed to display user position and indoor map.
If you want to customize it, with different features or design, follow this step to integrate, Nextome Flutter Map Source files.

1. Configure Flutter Sdk and prepare Android Studio with flutter plugins
 [Flutter Get Started](https://flutter.dev/docs/get-started/install)

2. Clone into your main project's folder, Nextome Flutter Map respository
 [Repository](https://github.com/Nextome/flutter_mapview)

3. Open flutter_mapview module with Android Studio and run it on iOS simulator.
 After finish, you're now ready to integrate modules into iOS main App.

4. Add to your PodFile
 ```pod
 flutter_application_path = '{path}/flutter_mapview'
 load File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')
 install_all_flutter_pods(flutter_application_path)
 ```
 Run pod install

5. Into your ***AppDelegate*** import and init Flutter engine
 ```swift
 import Flutter
 import FlutterPluginRegistrant
 ```
 ```swift
 var flutterEngine : FlutterEngine?
 ```
 After didFinishLaunchingWithOptions
 ```swift
 self.flutterEngine = FlutterEngine(name: "io.flutter", project: nil)
 self.flutterEngine?.run(withEntrypoint: nil)
 GeneratedPluginRegistrant.register(with: self.flutterEngine!)
 ```
6. Into your ***Main View Controller*** prepare flutter channel, to connect iOS and Flutter Engine.
 ```swift
 import Flutter
 ```

 ```swift
 var flutterViewController: FlutterViewController?
 var flutterEngine = (UIApplication.shared.delegate as? AppDelegate)?.flutterEngine
 var mapChannel = FlutterMethodChannel()
 ```
 ```swift
 flutterViewController = FlutterViewController(engine: flutterEngine!, nibName: nil, bundle: nil)
 self.mapChannel = FlutterMethodChannel(name: "net.nextome.phoenix", binaryMessenger: flutterViewController!.binaryMessenger)
 ```
 ***DONE***

## Observers
Open your ***Main View Controller*** and into viewDidLoad, attach to main's observers

```swift
//position observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceivePosition(_:)), name: NSNotification.Name(rawValue: "POSITION_STREAM"), object: nil)

//position observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveFloorChange(_:)), name: NSNotification.Name(rawValue: "FLOOR_CHANGE"), object: nil)

//path observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceivePath(_:)), name: NSNotification.Name(rawValue: "PATH_STREAM"), object: nil)

//outdoor observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveOutdoor(_:)), name: NSNotification.Name(rawValue: "OUTDOOR_STREAM"), object: nil)
```

(optional)
```swift
//log observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveLogs(_:)), name: NSNotification.Name(rawValue: "LOG_WRITER"), object: nil)

//error observer
NotificationCenter.default.addObserver(self, selector: #selector(onDidReceiveError(_:)), name: NSNotification.Name(rawValue: "ERROR"), object: nil)

//Refresh Poi observer
NotificationCenter.default.addObserver(self, selector: #selector(onRefreshPoiComplete(_:)), name: NSNotification.Name(rawValue: "REFRESHPOI_COMPLETE"), object: nil)
```
Prepares for use, onDidReceive functions, to manage Sdk data.
### onDidReceivePosition

```swift
@objc func onDidReceivePosition(_ notification: Notification){

 //receive position
 let positionData = notification.userInfo as? [String : [String: Any]]

 //get data
 let x = positionData!["positionData"]!["x"] as! String
 let y = positionData!["positionData"]!["y"] as! String
 let floor = positionData!["positionData"]!["floor"] as! String

 //send to flutter engine, new position's data
 self.mapChannel.invokeMethod("position", arguments: x + "," + y)

}
```

### onDidReceivePath

```swift
//MARK: MAP Path observer
@objc func onDidReceivePath(_ notification: Notification){
        
    //receive path
    let pathData = notification.userInfo as? [String : String]
        
    if(pathData!["pathData"] != nil){
        if(pathData!["pathData"] == "[]"){
            //advise ui
            PKHUD.sharedHUD.contentView = PKHUDTextView(text: "Shortest path not found. Please try again")
            PKHUD.sharedHUD.show()
            PKHUD.sharedHUD.dimsBackground = false
            PKHUD.sharedHUD.hide(afterDelay: 2) { success in
            }
        }else{
            self.mapChannel.invokeMethod("path", arguments: pathData!["pathData"]!)
        }
            
    }
}
```

### onDidReceiveOutdoor

```swift
@objc func onDidReceiveOutdoor(_ notification: Notification){

 //receive position
 let positionData = notification.userInfo as? [String : Double]

 //get data
 let lat = positionData!["lat"]!
 let lng = positionData!["lng"]!
}
```

### onDidReceiveFloorChange
```swift
@objc func onDidReceiveFloorChange(_ notification: Notification){
 //receive position
 let floorData = notification.userInfo as? [String : [String: Any]]

 //update poi
 let poiData = self.sdk?.getPOIData()
 mapChannel.invokeMethod("POI", arguments: poiData)

 //update map's package
 let data = self.sdk?.getVenueData()
 mapChannel.invokeMethod("localPackageUrl", arguments: data)

}
```

(optional)
### onDidReceiveError
```swift
@objc func onDidReceiveError(_ notification: Notification){
 //advise user
 print("Errore. Riavvia l'sdk ")

}
```

### onDidReceiveLogs
```swift
@objc func onDidReceiveLogs(_ notification: Notification){
 //get data
 let logInfo = notification.userInfo as? [String: String]
 var log = logInfo!["log"]!
}
```
### onRefreshPoiComplete
```swift
@objc func onRefreshPoiComplete(_ notification: Notification){
 //get new data and send to flutter engine
 let poiData = self.sdk?.getPOIData()
 mapChannel.invokeMethod("POI", arguments: poiData)
}
```

## Sdk Public Methods
### Start
Start Sdk
```swift
 sdk?.start()
```
### Stop
Stop Sdk
```swift
 sdk?.stop()
```
### Venue data (Flutter Utils)
Get current venue package data to easily send it, to flutter engine
```swift
 sdk?.getVenueData() -> String?
```
### POI (Flutter Utils)
Get current venue POI data to easily send it, to flutter engine
```swift
 sdk?.getPOIData() -> String?
```
### Refresh POI (Flutter Utils)
Force Refresh current venue POI data to easily send it, to flutter engine
```swift
 sdk?.refreshPOI()
```
### Refresh Map 
Force Refresh current displayed map
Keep Attention: if you force mapId during localization, sdk locks forced map and not switchs anymore
```swift
 sdk?.forceRefreshMap(mapId: Int)
```
To restore map's switch, according to user position
```swift
 sdk?.setLiveMap()
```

### Way Finding 
Calculate shortest route:
- from user position to selected map's POI
- from custom position A to custom position B
and send it to flutter engine
```swift
 sdk?.calculatePath_ToPOI(poi: String)
```
```swift
 sdk?.calculatePath_ToCustomPoint(point: String)
```
### Position
Get current user position data
```swift
 sdk?.getPositionData() -> [String: Any]
```
### Report
send SDK Report to Nextome admin
```swift
 sdk?.sendLogReport -> Void
```

## Flutter Map
Easily show flutter_map module with current venue (localized place) data
```swift
 //push flutter controller
 self.navigationController?.pushViewController(flutterViewController!, animated: true)

 //get map's poi
 let poiData = self.sdk?.getPOIData()
 mapChannel.invokeMethod("POI", arguments: poiData)

 //get map's resources
 let data = self.sdk?.getVenueData()
 mapChannel.invokeMethod("localPackageUrl", arguments: data)
 
 //flutter callbacks
 mapChannel.setMethodCallHandler{(call: FlutterMethodCall, result: FlutterResult) -> Void in
    // Handle poi json
    if call.method == "poiData"{
        //calculate path
        self.sdk?.calculatePath(poi: call.arguments as? String)
    }
            
    // Handle long press
    if(call.method == "customPosition"){
        let customPosition = call.arguments as! String
                
        //advise user
        if(self.sdk?.customPositionCheck == 0){
            //advise ui
            PKHUD.sharedHUD.contentView = PKHUDTextView(text: "Start point is set")
            PKHUD.sharedHUD.show()
            PKHUD.sharedHUD.hide(afterDelay: 1.0) { success in
            }
        }
                
        self.sdk?.calculateCustomPath(point: customPosition)

    }
 }
```

## Example
You can clone this repository and explore our complete Example project, to see how to manage all Sdk's data.

## App Distribution
If you have problem with app export from xcode or AppStore distribution, follow this steps:

1) Open your Xcode project's setting
2) Click on Build Phase tab
3) Add new Run Script and paste this code, which remove all x86 files and architecture from project
```sh
# Type a script or drag a script file from your workspace to insert its path.
 APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"

find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")

FRAMEWORK_TMP_PATH="$FRAMEWORK_EXECUTABLE_PATH-tmp"

case "${TARGET_BUILD_DIR}" in
*"iphonesimulator")
    echo "No need to remove archs"
    ;;
*)
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "i386") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "i386" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "i386 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "x86_64") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "x86_64" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "x86_64 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    ;;
esac

echo "Completed for executable $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")

done
```

### Connect with us

 https://www.nextome.net/

License
----

MIT

**© 2020 Nextome srl | All Rights Reserved.**
