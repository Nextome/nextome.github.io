# Prerequisites

# Aim
Install the technology that allows to locate the position of the smartphone on which the 'Nextome' application is installed

# Components

1)	Beacon sensors working as anchor references

2)	‘Venue ConFiguretor’ application to be installed on the smartphone used to install the technology

3)	‘Nextome’ application to be installed on each smartphone to be localized

# Installation phases
## Phase 1
Download the application ‘Venue Configurator’ that is needed to execute the installation.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/1.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 1: Download the ‘Venue Configurator’ app.</figcaption></p>


Login with the credential that are released by Nextome.

## Phase 2
Placement and registration of Beacon sensors playing the role of referencere anchors using the ‘Venue ConFigurator’ application.
Search and select the right venue among those that are registered.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/2.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 2: Search for the right Venue.</figcaption></p>

Select the right floor map among those that are registered for each venue.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/3.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 3: Floor selection.</figcaption></p>

Place each Beacons sensors in the positions that are suggested by Nextome.
For each sensor:

- Tap on the exact position on the map, this will activate the scanner of the smartphone, so that the latter will be ready to detect the right Beacon.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/4.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 4: map on which the Beacons will be registered.</figcaption></p>


- The scanner will detect the Beacons that are close to it. All the detected Beacons will appear in form of a list. Keeping the Beacons as close as possible to the smartphone (see following figure) while it is in “reception” mode will make the scanner able to detect the Beacons with lower Received Signal Strength Indicator (RSSI) values. The higher the RSSI value, the closer the Beacon.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/5.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 5: selecting the right Beacon from the list.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/6.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 6: list of detected Beacons.</figcaption></p>

As shown in previous figure, the list of the Beacons that are detected may have multiple items. Of course, the bigger the venue, the larger than number of Beacons detected at the same time. As it can be seen, the number in the circle on the left side of each item represents the strength of the signal that is received by the RSSI detector. Be sure to select the Beacon that is closer to the smartphone while you are as far as possible from the other Beacons (see following figure): it will appear on top of the list or, in any case, it will be the one with the higher value.
After selecting the right Beacon, tag on it and use the V icon on the upper right corner.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/7.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 7: selection of the Beacon.</figcaption></p>

Is the registration procedure is successful, the following message will be displayed: ‘Beacon added’.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/8.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 8: successful Beacon registration.</figcaption></p>

A blue icon will be displayed on the map in the selected release point.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/9.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 9: registered Beacons displayed.</figcaption></p>

After registering the Beacon, it will be possible to place the sensor on the ceiling or on the walls in the corresponding point of the venue. For better results, the Beacons should be installed at about 3 m height (in case of lower ceilings, place the Beacons as high as possible). It is important to try to install the beacons all at the same height because the algorithm expects the heights to be equal. For installation purposes, please consider using screws or tape.

## Phase 3
Download and execution of the ‘Nextome Indoor Positioning’ application, that is needed for the calculation, visualization, and memorization (if needed) of the position onboard the smartphone placed inside the venue.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/10.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 10: Download the 'Nextome Indoor Positioning' application from the App Store (Android or iOS).</figcaption></p>

Open the app and enable the required permissions on the following screens.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/11.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 11: 'Location background' permission screen.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/12.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 12: location access permission screen.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/13.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 13: geolocation access permission screen.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/14.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 14: Bluetooth activation screen.</figcaption></p>

Once all permissions are accepted, it is possible to log in and activate localization by selecting 'Enter' in the screen shown in following figure.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/15.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 15: Initial screen of the 'Nextome Indoor Positioning' application.</figcaption></p>

After a search session of a few seconds (following figure) the app will independently identify the map where the smartphone is located, and it will be possible to visualize the identified position of the smartphone by means of a blue spot placed inside the map.
<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/16.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 16: map search screen.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/17.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 17: identified position on the map.</figcaption></p>

By default, positions are only displayed on the map. In case positions should be saved on the database, it would be needed to set on ‘yes’ the “Send positions to server” option that is available in the settings menu of the application. In particular, as the application starts, as in Figure 18, use the upper right icon to access the Setting section and flag the option as in Figure 19.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/18.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 18: Splash screen of the 'Nextome Indoor Positioning' application.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/19.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 19: Settings with “Send position to Server” option selected.</figcaption></p>

Once the positions are saved on the database, it is possible to access them by means of dedicated APIs that are described in a dedicated document. It is worth noting that positions on the database are subject to a daily cancelation, so the available positions are related to the current day. The access to prior positions can be granted by Nextome if this is explicitly required to Nextome, together with the desired period.

### Phase 3.1
If you want to use the navigation service, you must first add the 'POIs' and ‘Paths’ through the 'Nextome Hub web' portal.
The POIs, i.e. the points to which navigation can be enabled, will therefore be viewable and navigable within the 'Nextome Indoor Positioning' app, in the following two ways:

1.	Click on the pin icon on the map (Figure 20) and start navigation by selecting 'Naviga' in the screen in Figure 21
2.	Semantic search of the POI (Figure 22) which can be accessed by selecting the item 'POI list' in the screen in Figure 23.

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/20.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 20:  POI icon display on map.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/21.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 21: POI's pop-up.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/22.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 22: semantic search of the POI.</figcaption></p>

<p style="text-align: center;"><img src="/assets/Installation guide for smartphone based localization/23.jpeg" width="40%"><figcaption style="text-align: center;"> Figure 23: access to the POI list.</figcaption></p>








