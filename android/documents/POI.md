# Advertisement / POIs

Advertisement and POI (Point of Interest) provide user to request the information of nearby POIs and all POIs.

## Class of POI

This structure is same as POI setting on server.
 
```java
public class POI {
    private int id = 0;
    private String contentType = null;
    private String contentURL = null;

    // Location of a POI only for outdoor case, or the value will be NaN.
    private Float poiLatitude = Float.NaN;
    private Float poiLongitude = Float.NaN;

    // Coordinate of a POI only for indoor case, or the value will be NaN.
    private Float coordX = Float.NaN;
    private Float coordY = Float.NaN;
    private Float coordZ = Float.NaN;

    // The angle of the POI on different axises. (Might be NaN)
    private Float eulerAngleX = Float.NaN;
    private Float eulerAngleY = Float.NaN;
    private Float eulerAngleZ = Float.NaN;
}
```
---

## Functions of POIRequester

Use ***POIRequester*** to request all or nearby POIs, and then give ***POICallbackListener*** to get results.  


- **POICallbackListener**  
To get POI requester callback. If *POIRequester* called api successfully, *onPOIRequestSuccess* will be called, and then you can get a list of POIs, or null when there is no POI belongs to the key. *onPOIRequestFailed* will be called when requester failed to get data. This might be because of network problem.

```java
interface POICallbackListener {
    void onPOIRequestSuccess(@Nullable List<POI> pois);

    void onPOIRequestFailed(Throwable throwable);
}
```

- **requestAllPOIs**  
For requesting all POIs from server by the key.

```java
public void requestAllPOIs(
            @NonNull String key,
            @Nullable final POICallbackListener callbackListener)
```

- **requestNearbyPOI**  
If you want to get all the *outdoor* nearby POIs, give latitude and longitude as arguments. Use map-world-coordinate to get *indoor* nearby POIs. The approximate distance in meters.

```java
// for outdoor nearby POIs
public void requestNearbyPOI(
            @NonNull String key,
            @NonNull Float lat,
            @NonNull Float lon,
            float distance,
            @Nullable final POICallbackListener callbackListener)
```  

```java
// for indoor nearby POIs
public void requestNearbyPOI(
            @NonNull String key,
            @NonNull Float coordX,
            @NonNull Float coordY,
            @NonNull Float coordZ,
            float distance,
            @Nullable final POICallbackListener callbackListener)
```

---

## Example

Simple use cases will look something like this:

```java
POIRequester requester = new POIRequester();
// all POIs
requester.requestAllPOIs(key, listener);
// outdoor POIs
requester.requestNearbyPOI(key, 26.041586f, 121.556511f, 2f, listener);
// indoor POIs
requester.requestNearbyPOI(key, 0f, 1f, 0f, 2f, listener);


private POICallbackListener listener = new POICallbackListener() {
    @Override
    public void onPOIRequestSuccess(@Nullable List<POI> pois) {
        // do something ...
    }
    @Override
    public void onPOIRequestFailed(Throwable throwable) {
        // do something ...
    }
};
```