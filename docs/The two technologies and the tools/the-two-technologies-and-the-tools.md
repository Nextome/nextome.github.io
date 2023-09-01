---
title: The two technologies and the tools
hide:
  - footer
---
 
# The two technologies and the tools

## Prerequisites
It is recommended to read this document before the others to get an overview of the main Nextome technologies and choose the one that best suits your use case.

## The two technologies
The two main Nextome Indoor localization systems are based on **Bluetooth technology** and consist of:

- Smartphone tracking --> which allows the localization of the smartphone on which an application is installed.
- Tag tracking --> which enables the localization of small battery-powered tags.

### Smartphone tracking
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>  

To calculate the position of a smartphone, it's necessary to install a mobile app on it that integrates the Nextome localization **SDK**. Thanks to this SDK, the smartphone can calculate its own position by processing the Bluetooth signals present in the environment. The signals considered by the localization app will only be those coming from the Beacons installed in the environment and registered within the Nextome Hub as **reference anchor Beacons**.

<p style="text-align: center;"><img src="/assets/Technologies and tools/1.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 1: Smartphone tracking architecture.</font> </figcaption></p>

### Tag tracking
<span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

To calculate the position of an object or a person, it's possible to associate a battery-powered Bluetooth **tag** with it. The Bluetooth signals transmitted by the tag will be detected by Bluetooth antennas (**gateways**) installed in the environment. These signals will be processed by the server to determine the position of the tag relative to the map of the environment it is in. The signals considered by the calculation engine will only be those transmitted and detected by the tags and gateways registered within the Nextome Hub.

<p style="text-align: center;"><img src="/assets/Technologies and tools/2.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 2: Tag tracking architecture.</font> </figcaption></p>

## The tools 
This section will introduce the tools that allow interfacing with the Nextome Hub, providing the ability to utilize the technologies described above.

<p style="text-align: center;"><img src="/assets/Technologies and tools/3.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 3: Overview tools.</font> </figcaption></p>

### Nextome Hub
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

The Nextome Hub is the core of the Nextome system, housing the localization engine, all the necessary information for calculation, and the history of calculated data where required.

### The interfaces
To interface with the Nextome Hub, you can use the Nextome Hub Web and the Venue Configurator. These have been developed with a user-friendly interface by the Nextome team. Alternatively, you can directly use the API calls that allow integrating all functionalities into other systems.

#### Nextome Hub web
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

The Nextome Hub Web is a web portal that allows:

- Management of maps and all the resources necessary for the operation of the technologies.
- Monitoring the status of the hardware installed in the environment.
- Visualization of calculated positions.

#### Venue Configurator
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

The Venue Configurator is a smartphone app that allows for the easy registration of hardware installed in the environment. It is also possible to register hardware through the Nextome Hub Web. However, the Venue Configurator simplifies and expedites the process, offering additional installation verification capabilities by leveraging the smartphone's scanner functionality.

#### Hub APIs
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

The API calls allow you to perform all the necessary configuration operations for the technology's operation and to download calculated positions (functionalities also available in the Nextome Hub Web and the Venue Configurator).

### The services

#### Sdk and Test App
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span> 

Nextome provides an SDK that can be integrated into native applications (Android, iOS), enabling the functionality of smartphone localization for devices where it's installed.

To test the functionality of smartphone localization, the 'Nextome Indoor Positioning app' is ready for use. This app allows users to visualize their position on a map and utilize all the associated services, such as navigating to points of interest or receiving notifications when approaching specific geo-fenced areas. Prior to using the app, it's necessary to install and register Beacons in the environment and configure additional resources through the Venue Configurator, Nextome Hub Web, or APIs.

#### Map view
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

The Map View is an integrable component for native applications (Android, iOS) that provides the capability to display two-dimensional environment represented by maps, such as floor plans. On these maps, it's possible to visualize "customizable" additional information like markers, paths, and polygons (or shapes). Specifically, markers are images uploaded by the user that can represent specific point-based information, such as a Point of Interest (POI).





