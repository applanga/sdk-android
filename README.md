#Applanga SDK for Android
***
*Version:* 1.0.0

*URL:* <http://applanga.com> 
***

##Installation
1. Download the latest release of the Applanga Android SDK from [Github](https://github.com/applanga/sdk-android/releases). Unzip it, then drag and drop Applanga.aar into your project's app/libs folder. **Note**: if you are using drag and drop on OSX you may need to hold down the option key.


2. Add the Applanga SDK to your list of dependencies in your **build.gradle** script.

        compile(name:'Applanga', ext:'aar')

3. Applanga is using [retrofit](http://square.github.io/retrofit/) and [OkHttp](http://square.github.io/okhttp/) for Network Communication so you need to add them to your list of dependencies in your **build.gradle** script as well as the android.support and appcompat libraries.

        dependencies {
          compile fileTree(dir: 'libs', include: ['*.jar'])
          compile 'com.android.support:support-v4:20.0.0'
          compile 'com.android.support:appcompat-v7:20.0.0'
          compile 'com.squareup.retrofit:retrofit:1.6.1'
          compile 'com.squareup.okhttp:okhttp-urlconnection:2.0.0'
          compile 'com.squareup.okhttp:okhttp:2.0.0'
          compile(name:'Applanga', ext:'aar')
        }

4. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your App internet access, which is needed for Applanga to function. If you want to track device names you also need to add **android.permission.BLUETOOTH** in the manifest.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.BLUETOOTH" />

5. If you are using [ProGuard](http://developer.android.com/tools/help/proguard.html) add the following lines for Applanga, retrofit and OkHttp to your proguard file.

        -libraryjars libs
        -keepattributes Signature
        -keepattributes *Annotation*

        -dontwarn com.squareup.okhttp.**
        -dontwarn rx.**
        -dontwarn retrofit.**
        -dontwarn okio.**

        -keep interface com.squareup.okhttp.** { *; }
        -keep class com.squareup.okhttp.** { *; }
        -keep class sun.misc.Unsafe { *; }
        -keep class retrofit.** { *; }
        -keepclasseswithmembers class * {
          @retrofit.http.* ;
        }
        
        -keep public class com.applanga.android.Applanga
        -keepclassmembers public class com.applanga.android.Applanga {
          *;
        }
        
        -keep class com.applanga.android.ApplangaCallback
        -keepclassmembers class com.applanga.android.ApplangaCallback {
          *;
        }
        
        -keep public class com.applanga.android.request.ApiResult
        -keepclassmembers class com.applanga.android.request.ApiResult {
          *;
        }

##Configuration
1. Download the Applanga *settingsfile* for your app from the Applanga App Overview by clicking ***[download settings]***. (The filename should reflect your bundle id or package name as lowercase string and dots replaced with underscores with the ending .applanga)
 
2. Add the *settingsfile* to your apps resources res/raw directory.


##Usage
Once Applanga is integrated and configured it is synchronizing your local strings with the Applanga dashboard every time you start your app or if new missing strings are found.

1. Import **Applanga** package in your Main Activity.

        import com.applanga.android.Applanga;
        import com.applanga.android.ApplangaCallback;

2. Create a class that implements the **ApplangaCallback** Interface to get notified when the data update is finished.

        public class MyApplangaCallback implements ApplangaCallback {

          @Override
          public void onSuccess() {
            // do something useful like loading your UI.
          }

          @Override
          public void onFailure() {
            // do something useful like loading your UI.
          }
        }

3. Configure **Applanga** by specifying the App Token of your app in the onCreate method of your Main Activity.

        Applanga.configure(this, R.raw.com_YOURSETTINGSFILE, new MyApplangaCallback());

3. Call the following API function to get a localized version of a string based on the current device locale.

        Applanga.localizedStringForKey("KEY");
