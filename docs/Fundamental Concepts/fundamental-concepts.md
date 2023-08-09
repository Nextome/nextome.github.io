# Prerequisites
In this document, all the definitions of the fundamental concepts within the Nextome Hub are provided. The content of this document is essential for understanding the meaning of the documents:
-   Nextome Hub Web Guide
-   Venue Configurator Guide
-   SDK Documentation 
-   API Documentation
-   Map View and Flutter Map Documentation
-   Tag Tracking Installation Guide
-   Smartphone Tracking Installation Guide

The foundational documents for understanding this document are:
-   The two technologies
-   The tools

# Introduction and Context
For the operation of the two Nextome technologies, the following steps are necessary:
1. Creation within the Nextome Hub of the 'Venue' resource, which represents the area where the technology is installed.
2. Creation within the Nextome Hub of 'Map' resources, one for each floor, and association with the corresponding Venue.
3. Physical installation of hardware within the structure.
4. Registration of the installed hardware within the Nextome Hub.
Once the above steps are completed, the technology is capable of calculating the 'Positions' of the items being localized, which can be a 'Tag' or a 'Device' (smartphone).

To enable additional services beyond position calculation, such as navigation or geofencing, the following is necessary:
- Create additional resources on the relevant 'Maps' within the Nextome Hub.

The concepts introduced in points 1 and 2 will be explored in-depth in the 'Concepts related to the installation site' section. The concepts introduced in points 3 and 4 will be presented and described in the 'Concepts related to hardware' section. The concepts introduced in point 5 will be presented and described in the 'Concepts related to additional services' section. The concepts related to the calculated positions from the localization engine will be explored in the 'Concepts related to calculated positions' section.

For each concept:
- It will be specified which technology it involves (tag tracking, smartphone tracking, or both).
- The context in which it is used will be briefly explained.
- A definition will be provided based on its primary characteristics.

In the following figure, a diagram is presented that illustrates the relationships between the various concepts.

**INSERIRE IMMAGINE**

# Concepts related to the location where the technology is installed

## Venue
:cellphone-cog: Smartphone Tracking 
:tag: Tag Tracking 
*Definition*
The concept of 'Venue' represents the location where the Nextome technology is installed. 

For each Venue, at least one 'Map' must be associated, referring to a single floor within the structure. If there are multiple floors, there will be multiple Maps associated with the Venue.

The concept of Venue is linked to the timezone to be associated with the positions of devices calculated by the installation within that specific Venue.

Furthermore, since the functionality of the technology is configurable based on the Venue, the settings that the technology uses must be associated with the Venue.

The Venue concept can also be associated with an address and latitude and longitude coordinates for georeferencing purposes.
