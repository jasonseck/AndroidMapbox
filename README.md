# AndroidMapbox
Native Mapbox Navigation in React Native Android

Mapbox/Android setup:

Add Mapbox source and implementation to build.gradle(s)
maven { url 'https://mapbox.bintray.com/mapbox' }
implementation 'com.mapbox.mapboxsdk:mapbox-android-navigation-ui:0.40.0'

Add permissions to AndroidManifest.xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

Add Mapbox token to res.values.strings.xml
<string name="mapbox_access_token">YOUR TOKEN HERE/string>

Create empty activity for the navigation to exist within.
(MapboxActivity) - also creates layout res.layout.activity_mapbox.xml

Add a spot in the res.layout.activity_mapbox.xml for the navigation to reference.
...
    <com.mapbox.services.android.navigation.ui.v5.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />
...	

Create a react native turn-by-turn navigation module( RNTbTNavigationModule )
-this contains the methods that get exposed on the react native side.
-next step is to pass origin and destination variables to the intent.

Create a react native turn-by-turn navigation package (RNTbTNavigationPackage)
-this packages the modules together, if you have more than one, for example an event emitter.

In MainApplication, add the new package...
...
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
      new RNTbTNavigationPackage()
      );
    }
...

In MapboxActivity
-in the main class, implement OnNavigationReadyCallback, NavigationListener
-create and initialize navigationView
-@override all mapbox functions, or you'll get crashes..
-setup a function to build the route and launch the navigationview

On react native / js side
Call nativemodules.RNTbTNavigation.mapView();

