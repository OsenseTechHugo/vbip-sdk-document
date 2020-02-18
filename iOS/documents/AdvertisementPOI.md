# Advertisement / POI

----------

## Overview

Advertisement and POI (Point of Interest) provide user to request the information of nearby POIs and all POIs.

## Functions

```objective-c
struct OsensePOI{
    string ID; // ID of the POI.
    string contentType; // The type of AR content.
    string contentURL; // The URL of AR content.
    float poiLatitude; // The latitude of the POI. (Only for outdoor case, or it will be NAN)
    float poiLongitude; // The longitude of the POI. (Only for outdoor case, or it will be NAN)
    float coordX; // The x-coordinate of the POI. (Only for indoor case, or it will be NAN)
    float coordY; // The y-coordinate of the POI. (Only for indoor case, or it will be NAN)
    float coordZ; // The z-coordinate of the POI. (Only for indoor case, or it will be NAN)
    float eulerAngleX; // The angle of the POI on the x-axis. (It can be NAN)
    float eulerAngleY; // The angle of the POI on the y-axis. (It can be NAN)
    float eulerAngleZ; // The angle of the POI on the z-axis. (It can be NAN)
};
```

```objective-c
(void) requestAllPOI:(string) key complete:(void(^)(vector<OsensePOI> allPOI))completion
```
- Request all POIs.

  * `key` - Pass the verification code.
  * `allPOI` - Return a vector that contains all the POIs in the map.


```objective-c
(void) requestIndoorNearbyPOI:(string) key 
                   withCoordX:(float) coordX 
                   withCoordY:(float) coordY 
                   withCoordZ:(float) coordZ 
                     withDist:(float) distance 
                     complete:(void(^)(vector<OsensePOI> nearPOI))completion
```
- Request indoor POIs nearby the user.

  * `key` - Pass the verification code.
  * `coordX` - User's x-coordinate in the map.
  * `coordY` - User's y-coordinate in the map.
  * `coordZ` - User's z-coordinate in the map.
  * `distance` - Request POIs within the distance.
  * `nearPOI` - Return a vector that contains POIs near user in the map.


```objective-c
(void) requestOutdoorNearbyPOI:(string) key 
                       withLat:(float) latitude 
                      withLong:(float) longitude 
                      withDist:(float) distance 
                      complete:(void(^)(vector<OsensePOI> nearPOI))completion
```
- Request outdoor POIs nearby the user.

  * `key` - Pass the verification code.
  * `latitude` - User's latitude.
  * `longitude` - User's longitude.
  * `distance` - Request POIs within the distance.
  * `nearPOI` - Return a vector that contains POIs near user in the map.


## Examples

The sample code of requesting all POIs:
```objective-c
POI *askAllPOI = [[POI alloc] init];
    [askAllPOI requestAllPOI:"verificationCode" complete:^(vector<OsensePOI> allPOI) {
        for (int i = 0; i < allPOI.size(); i++) {
            NSLog(@"ID = %@\ncontentType = %@\ncontentURL = %@\nlatitude = %f, longitude = %f, coordX = %f, coordY = %f, coordZ = %f, eulerX = %f, eulerY = %f, eulerZ = %f",
            [NSString stringWithCString:allPOI[i].ID.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:allPOI[i].contentType.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:allPOI[i].contentURL.c_str() encoding:[NSString defaultCStringEncoding]],
            allPOI[i].poiLatitude,
            allPOI[i].poiLongitude,
            allPOI[i].coordX,
            allPOI[i].coordY,
            allPOI[i].coordZ,
            allPOI[i].eulerAngleX,
            allPOI[i].eulerAngleY,
            allPOI[i].eulerAngleZ);
        }
    }];
```
The results are printed below:
```
ID = 1
contentType = Video
contentURL = https://osensetech.blob.core.windows.net/ads/V1.mp4
latitude = 26.041586, longitude = 121.556511, coordX = nan, coordY = nan, coordZ = nan, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 2
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
latitude = 25.039551, longitude = 121.560265, coordX = nan, coordY = nan, coordZ = nan, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 3
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
latitude = nan, longitude = nan, coordX = 3.000000, coordY = 0.000000, coordZ = -2.000000, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 6
contentType = Picture
contentURL = https://osensetech.blob.core.windows.net/btsdemo/ChitLom1F.jpg
latitude = nan, longitude = nan, coordX = 1.000000, coordY = 1.000000, coordZ = 0.000000, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 7
contentType = Voice
contentURL = https://osensetech.blob.core.windows.net/ads/Splashing_Around.mp3
latitude = nan, longitude = nan, coordX = 3.000000, coordY = 0.000000, coordZ = -2.000000, eulerX = nan, eulerY = nan, eulerZ = nan
```

The sample code of requesting indoor POIs near user:
```objective-c
POI *askNearbyIndoorPOI = [[POI alloc] init];
    [askNearbyIndoorPOI requestIndoorNearbyPOI:"verificationCode" 
                                    withCoordX:indoorPosition.coordX
                                    withCoordY:indoorPosition.coordY
                                    withCoordZ:indoorPosition.coordZ
                                      withDist:5 complete:^(vector<OsensePOI> nearPOI) {
        for (int i = 0; i < nearPOI.size(); i++) {
            NSLog(@"ID = %@\ncontentType = %@\ncontentURL = %@\ncoordX = %f, coordY = %f, coordZ = %f, eulerX = %f, eulerY = %f, eulerZ = %f",
            [NSString stringWithCString:nearPOI[i].ID.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:nearPOI[i].contentType.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:nearPOI[i].contentURL.c_str() encoding:[NSString defaultCStringEncoding]],
            nearPOI[i].coordX,
            nearPOI[i].coordY,
            nearPOI[i].coordZ,
            nearPOI[i].eulerAngleX,
            nearPOI[i].eulerAngleY,
            nearPOI[i].eulerAngleZ);
        }
    }];
```
The results are printed below:
```
ID = 5
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
coordX = 2.000000, coordY = 0.000000, coordZ = 0.000000, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 6
contentType = Picture
contentURL = https://osensetech.blob.core.windows.net/btsdemo/ChitLom1F.jpg
coordX = 1.000000, coordY = 1.000000, coordZ = 0.000000, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 10
contentType = Video
contentURL = https://osensetech.blob.core.windows.net/ads/V1.mp4
coordX = 0.000000, coordY = 0.000000, coordZ = -2.000000, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 35
contentType = Picture
contentURL = https://osensetech.blob.core.windows.net/btsdemo/ChitLom1F.jpg
coordX = -1.000000, coordY = 0.000000, coordZ = -1.000000, eulerX = nan, eulerY = nan, eulerZ = nan
```
The sample code of requesting outdoor POIs near user:
```objective-c
POI *askNearbyOutdoorPOI = [[POI alloc] init];
    [askNearbyOutdoorPOI requestOutdoorNearbyPOI:"verificationCode" 
                                         withLat:outdoorPosition.latitude
                                        withLong:outdoorPosition.longitude
                                        withDist:50 complete:^(vector<OsensePOI> nearPOI) {
        for (int i = 0; i < nearPOI.size(); i++) {
            NSLog(@"ID = %@\ncontentType = %@\ncontentURL = %@\nlatitude = %f, longitude = %f, eulerX = %f, eulerY = %f, eulerZ = %f",
            [NSString stringWithCString:nearPOI[i].ID.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:nearPOI[i].contentType.c_str() encoding:[NSString defaultCStringEncoding]],
            [NSString stringWithCString:nearPOI[i].contentURL.c_str() encoding:[NSString defaultCStringEncoding]],
            nearPOI[i].poiLatitude,
            nearPOI[i].poiLongitude,
            nearPOI[i].eulerAngleX,
            nearPOI[i].eulerAngleY,
            nearPOI[i].eulerAngleZ);
        }
    }];
```
The results are printed below:
```
ID = 9
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
coordX = nan, coordY = nan, coordZ = nan, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 33
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
coordX = nan, coordY = nan, coordZ = nan, eulerX = nan, eulerY = nan, eulerZ = nan

ID = 34
contentType = Model
contentURL = https://osensetech.blob.core.windows.net/ads/iconC.scn
coordX = nan, coordY = nan, coordZ = nan, eulerX = nan, eulerY = nan, eulerZ = nan
```