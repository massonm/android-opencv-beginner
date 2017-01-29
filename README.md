	
Android and OpenCV (= Open Source Computer Vision)
==================================================
In order to learn to develop an Android app, we need an "idea" to implement...  
and a funny way may be to use OpenCV library and camera phone ^_^


	
Installation
------------
Tools and software components available at this time (_**January 2017**_):

1. Android Studio v2.2.3:  
   [download Android Studio](https://developer.android.com/studio/index.html)  
   [installation steps](https://developer.android.com/studio/install.html)  
2. OpenCV for Android v3.2.0 (2016-12-23)  
   [download OpenCV for Android](http://opencv.org/downloads.html)  
   /!\\ OpenCV for Android installation steps are **deprecated** because based on _Eclipse Android Development Tools (ADT)_  
       [Now _Eclipse ADT_ has been replaced by _Android Studio_](https://developer.android.com/studio/tools/sdk/eclipse-adt.html)  
   These steps are deprecated but some information may still be helpful:  
   [DEPRECATED steps](http://opencv.org/platforms/android.html)  
   For installation, just unzip SDK archive into your local folder.  
3. OpenCV Manager (see next chapters)



How to enable _USB debugging_ on phone ?
----------------------------------------
from: [https://developer.android.com/studio/run/device.html](https://developer.android.com/studio/run/device.html)

> "To access these settings, open the Developer options in the system Settings.  
> On Android 4.2 and higher, the Developer options screen is hidden by default.  
> To make it visible, go to Settings > About phone and tap Build number seven times.  
> Return to the previous screen to find Developer options at the bottom."



How to install _OpenCV Manager_ (Android app) ?
-----------------------------------------------
* in Google Play, we _**may**_ install _OpenCV Manager_ app  
  /!\\ official _OpenCV Manager_ in Google Play is **outdated** and runtime errors may occur when executing samples !?  
      (EXAMPLE: `java.lang.UnsatisfiedLinkError: Native method not found: org.opencv.imgproc.Imgproc.Canny_3`)  
  [OUTDATED OpenCV Manager](https://play.google.com/store/apps/details?id=org.opencv.engine)
* so instead, because of runtime errors, install _OpenCV Manager_ contained in OpenCV SDK:  
  check your hardware ABI name: [Android ABI names](https://developer.android.com/ndk/guides/abis.html)  
  see also: [OpenCV Android platform README file](https://github.com/opencv/opencv/blob/3.2.0/platforms/android/service/readme.txt)  
  in a console, execute these commands:  
    * check that USB debugging is working:  
      `<your folder>/Android/Sdk/platform-tools/adb devices -l`
    * install app (**/!\\ choose APK file that match your hardware ABI name**):  
      `<your folder>/Android/Sdk/platform-tools/adb install <your folder>/OpenCV-android-sdk/apk/OpenCV_3.2.0_Manager_3.20_armeabi-v7a.apk`



How to import _OpenCV for Android_ sample(s) in Android Studio ?
----------------------------------------------------------------
Even if _Eclipse ADT_ is deprecated, we can still import OpenCV sample(s) in Android Studio:

1. launch _Android Studio_
2. in first window (`Welcome to Android Studio` wizard), select: `Import project (Eclipse ADT, Gradle, etc.)`  
   (if not displayed, you have to close your current project: go to `File` menu then click on `Close Project`)
3. in new window, enter path to OpenCV SDK sample folder:  
   in this example, `image-manipulations` is used: `OpenCV-android-sdk/samples/image-manipulations`
4. in next window, enter your project destination path
5. in next window, leave all default options and press `Finish`
6. in new window, we should see:
      * `import-summary.txt` file, describing _ADT Eclipse_ to _Android Studio_ files conversion
      * in project panel:
         * `openCVLibrary320`
         * `openCVSampleImagemanipulations`
         * `Gradle Scripts`
7. **/!\\ some "manual" modifications will be done later (see next chapters)**



How to execute _OpenCV for Android_ sample(s) on phone ?
--------------------------------------------------------
1. enable _USB debugging_ on phone : see dedicated chapter
2. install _OpenCV Manager_ on phone : see dedicated chapter
3. try to compile and execute OpenCV sample on phone:  
   in _Android Studio_, go to `Run` menu then click on `Debug`  
   **/!\\ compilation errors may be triggered !!!**  
   EXAMPLE: `error: cannot find symbol variable GL_TEXTURE_EXTERNAL_OES`  
   In this case:
      * edit `build.gradle (Module: openCVSampleImagemanipulations)`:  
         * `buildToolsVersion "25.0.2"` should already be set
         * replace: `compileSdkVersion 11`  
           by: `compileSdkVersion 25`
      * idem for: `build.gradle (Module: openCVLibrary320)`
      * after these modifications, click on `Sync Now` (in the new gradle status popup)
4. Now application should be able to run on phone...  
   ... **/!\\ BUT** application's menu may **not** be displayed : see next chapter



Why _OpenCV for Android_ sample(s) menu may not be displayed on phone ?
-----------------------------------------------------------------------
Because menu is displayed thanks to _**legacy**_ `Action Overflow button` in `System/Navigation Controls bar`.  
**/!\\ This button has been deprecated in Android 4.0, that's why menu may not be displayed (on phones with hardware system bar for example) !**  
So menu shall be handled by an `App Bar` also known as `Action Bar` (_that is missing and needs to be created_).

* see: [Android Backwards Compatibility](https://developer.android.com/design/patterns/compatibility.html)
* from: [Say Goodbye to the Menu Button](https://android-developers.googleblog.com/2012/01/say-goodbye-to-menu-button.html)

  > "If you set either `minSdkVersion` or `targetSdkVersion` to `11 or higher`, the system will not add the `legacy overflow button`.  
  > Otherwise, the system will add the `legacy overflow button` when running on Android 3.0 or higher.  
  > The only exception is that if you set `minSdkVersion` to `10 or lower`, set `targetSdkVersion` to `11`, `12`, or `13`,  
  > and you do not use `ActionBar`, the system will add the `legacy overflow button` when running your app on a  
  > handset with Android 4.0 or higher.  
  > That exception might be a bit confusing, but it’s based on the belief that if you designed your app to support  
  > pre-Honeycomb handsets and Honeycomb tablets, it probably expects handset devices to include a Menu button (but  
  > it supports tablets that don’t have one).  
  > So, to ensure that the `overflow action button` never appears beside the `system navigation`, you should set the  
  > `targetSdkVersion` to `14`. (You can leave `minSdkVersion` at something much lower to continue supporting older devices.)"

* indeed, if we edit gradle scripts:
    * menu is displayed with:
        * `minSdkVersion 8`
        * `targetSdkVersion 13`
    * menu is not displayed with:
        * `minSdkVersion 8`
        * `targetSdkVersion 14`
    * SDK versions are based on **deprecated** fields in `AndroidManifest.xml`:
        * [`openCVLibrary320` Android SDK versions](https://github.com/opencv/opencv/blob/3.2.0/modules/java/android_lib/AndroidManifest.xml#L7)
        * [`openCVSampleImagemanipulations` Android SDK versions](https://github.com/opencv/opencv/blob/3.2.0/samples/android/image-manipulations/AndroidManifest.xml#L29)



Where to find OpenCV source files ?
-----------------------------------
* [master branch](https://github.com/opencv/opencv)
* [OpenCV's extra modules](https://github.com/opencv/opencv_contrib)
* [OpenCV Android SDK](https://github.com/opencv/opencv/tree/master/modules/java/android_lib)
* [OpenCV Android samples](https://github.com/opencv/opencv/tree/master/samples/android)
* [Android OpenCV Manager](https://github.com/opencv/opencv/tree/master/platforms/android)
* [(Eclipse ADT) OpenCV Android tutorials](https://github.com/opencv/opencv/tree/master/doc/tutorials/introduction/android_binary_package)



How to use webcam in Android Emulator ?
---------------------------------------
Follow steps in these forum posts:

* [Android: How to use webcam in emulator?](http://stackoverflow.com/questions/14012924/android-how-to-use-webcam-in-emulator)
* [Android Emulator: Unable to start webcam to capture picture in emulator](http://stackoverflow.com/questions/27875415/android-emulator-unable-to-start-webcam-to-capture-picture-in-emulator)



What can be changed in OpenCV Android sample(s) / how to learn Android app development ?
----------------------------------------------------------------------------------------
* use `Android Support Library` (assure backward compatibility, and needed for `app bar` see later...):
    * [Supporting Different Platform Versions](https://developer.android.com/training/basics/supporting-devices/platforms.html)
    
        > "Note: When parsing XML resources, Android ignores XML attributes that aren’t supported by the current device.  
        > So you can safely use XML attributes that are only supported by newer versions without worrying about older  
        > versions breaking when they encounter that code. For example, if you set the targetSdkVersion="11", your app  
        > includes the ActionBar by default on Android 3.0 and higher. To then add menu items to the action bar,  
        > you need to set android:showAsAction="ifRoom" in your menu resource XML. It's safe to do this in a  
        > cross-version XML file, because the older versions of Android simply ignore the showAsAction attribute (that is,  
        > you do not need a separate version in res/menu-v11/)."
    
    * [Support Library Features Guide](https://developer.android.com/topic/libraries/support-library/features.html)
    * [Support Library](https://developer.android.com/topic/libraries/support-library/index.html)
    * [Support Library Setup](https://developer.android.com/topic/libraries/support-library/setup.html)
* change menu implementation:
    * replace [`MenuItem attributes`](https://github.com/opencv/opencv/blob/3.2.0/samples/android/image-manipulations/src/org/opencv/samples/imagemanipulations/ImageManipulationsActivity.java#L133)
    * by XML ressource file in `res/menu/your_activity.xml`: [Android XML Menu](https://developer.android.com/guide/topics/ui/menus.html)
* centralize ressources in dedicated files:
    * styles in `res/values/styles.xml`: [Style Resource](https://developer.android.com/guide/topics/resources/style-resource.html)
    * strings in `res/values/strings.xml`: [String](https://developer.android.com/guide/topics/resources/string-resource.html)
    * layout in `res/layout/your_view.xml`: [Layouts](https://developer.android.com/guide/topics/ui/declaring-layout.html)
* increase `targetSdkVersion` to recent Android version, see also:
    * [API level](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)
    * [Android Plaform Versions in use](https://developer.android.com/about/dashboards/index.html)
* add an `app bar`:
    * [Adding the App Bar](https://developer.android.com/training/appbar/index.html)
    * [Setting Up the App Bar](https://developer.android.com/training/appbar/setting-up.html)
* link menu to this new `app bar`
* show/hide `app bar` when users touch the screen
* enable fullscreen mode:
    * [SYSTEM_UI_FLAG_FULLSCREEN](https://developer.android.com/reference/android/view/View.html#SYSTEM_UI_FLAG_FULLSCREEN)

        > "This approach to going fullscreen is best used over the window flag when it is a transient state -- that is,  
        > the application does this at certain points in its user interaction where it wants to allow the user to  
        > focus on content, but not as a continuous state. For situations where the application would like to simply  
        > stay full screen the entire time (such as a game that wants to take over the screen), the window flag is  
        > usually a better approach."

    * [Full Screen: Lean Back vs. Immersive](https://developer.android.com/design/patterns/fullscreen.html)
    * [Using Immersive Full-Screen Mode](https://developer.android.com/training/system-ui/immersive.html)
* add a new feature, like circle detection
* add another feature, like color filtering: filter current camera frame on a specific color range
* add object tracking feature
* add whatever feature you want ! ;)



How to create your own Android app based on OpenCV in Android Studio ?
----------------------------------------------------------------------
* first, create a new project in Android Studio:
    * launch _Android Studio_
    * in first window (`Welcome to Android Studio` wizard), select: `Start a new Android Studio project`  
      (if not displayed, you have to close your current project: go to `File` menu then click on `Close Project`)
    * choose your app `API level`
    * select: `Add No Activity`
* then, follow steps in these forum posts (**/!\\ but do not copy files into `app/src/main`, see above**):
    * [how i use opencv on android studio](http://answers.opencv.org/question/118335/how-i-use-opencv-on-android-studio/)
    * [OpenCV in Android Studio](http://stackoverflow.com/questions/27406303/opencv-in-android-studio#answer-27421494)

**/!\\ do not copy folder from `OpenCV-android-sdk/sdk/native/libs/` into `app/src/main` but into `app/` instead !**  
Otherwise your APK file will contain all these files (that should be handled by _OpenCV Manager_) and will **have a very big size** !  
Then do no forget to rename it into: `jniLibs`.

* see previous chapters about: API level, menu, Support Library, app bar, etc.


