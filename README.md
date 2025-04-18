# Save image to gallery 
(without storage permission headaches) 🤦‍♂️🤦‍♂️🤦‍♂️


This sample snippet is taken from the Flutter package: 
---
- saver_gallery 4.0.1: https://pub.dev/packages/saver_gallery
- dont forget to run `flutter pub get`

Add the following permissions to your **`AndroidManifest.xml`** file:

#### Location in Flutter:

`android/app/src/main/AndroidManifest.xml`

1. Add `xmlns:tools="http://schemas.android.com/tools"` to the `<manifest>` tag. like this:

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

```

2. Within the `manifest` block add these lines:

```
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" tools:ignore="ScopedStorage" />
<!-- Required if skipIfExists is set to true -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
```


Here's how it should look like in full: **[ example only ]**

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        tools:ignore="ScopedStorage" />
    <!-- Required if skipIfExists is set to true -->
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />

    <application
        android:label="save_image"
        android:name="${applicationName}"
        android:icon="@mipmap/ic_launcher">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:launchMode="singleTop"
            android:taskAffinity=""
            android:theme="@style/LaunchTheme"
            android:configChanges="orientation|keyboardHidden|keyboard|screenSize|smallestScreenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
            android:hardwareAccelerated="true"
            android:windowSoftInputMode="adjustResize">
            <!-- Specifies an Android theme to apply to this Activity as soon as
                 the Android process has started. This theme is visible to the user
                 while the Flutter UI initializes. After that, this theme continues
                 to determine the Window background behind the Flutter UI. -->
            <meta-data
                android:name="io.flutter.embedding.android.NormalTheme"
                android:resource="@style/NormalTheme"
            />
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <!-- Don't delete the meta-data below.
             This is used by the Flutter tool to generate GeneratedPluginRegistrant.java -->
        <meta-data
            android:name="flutterEmbedding"
            android:value="2" />
    </application>
    <!-- Required to query activities that can process text, see:
         https://developer.android.com/training/package-visibility and
         https://developer.android.com/reference/android/content/Intent#ACTION_PROCESS_TEXT.

         In particular, this is used by the Flutter engine in io.flutter.plugin.text.ProcessTextPlugin. -->
    <queries>
        <intent>
            <action android:name="android.intent.action.PROCESS_TEXT" />
            <data android:mimeType="text/plain" />
        </intent>
    </queries>
</manifest>
```

## NOTES:
```
# SAVE IMAGE TO GALLERY FIX [pubscpec.yaml]
saver_gallery: ^4.0.1
device_info_plus: ^11.3.3
permission_handler: ^12.0.0+1
fluttertoast: ^8.2.12

// SAVING IMAGE TO GALLERY [import dependencies]
import 'package:saver_gallery/saver_gallery.dart';
import 'package:device_info_plus/device_info_plus.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:tectags/services/shared_prefs_service.dart';


  @override 
  void initState() {
    _requestPermission(); // [gain permission]
  }

 /// Requests necessary permissions based on the platform.  // [gain permission]
  Future<void> _requestPermission() async {
    bool statuses;
    if (Platform.isAndroid) {
      final deviceInfoPlugin = DeviceInfoPlugin();
      final deviceInfo = await deviceInfoPlugin.androidInfo;
      final sdkInt = deviceInfo.version.sdkInt;
      statuses =
          sdkInt < 29 ? await Permission.storage.request().isGranted : true;
    } else {
      statuses = await Permission.photosAddOnly.request().isGranted;
    }
    debugPrint('Permission Request Result: $statuses');
  }

  final Uint8List? screenShot = await screenshotController.capture(); // [save your actual image]
  final result = await SaverGallery.saveImage(
        screenShot,
        fileName: "screenshot_${DateTime.now().millisecondsSinceEpoch}.png",
        skipIfExists: false,
      ); // [save your actual image] screenShot is my image

  debugPrint("Result: $result"); [check structure of: result]

if(result.isSuccess){} //  [do your checks]
```
