#Applanga SDK for Android
***
*Version:* 1.0.1

*URL:* <http://applanga.com> 
***

##Installation
1. Download the latest release of the Applanga Android SDK from [Github](https://github.com/applanga/sdk-android/releases). Unzip it, then drag and drop Applanga.aar into your project's app/libs folder. **Note**: if you are using drag and drop on OSX you may need to hold down the option key.

2. Add the Applanga SDK to your list of dependencies in your **build.gradle** script.

        compile(name:'Applanga', ext:'aar')

3. Applanga is using [retrofit](http://square.github.io/retrofit/) and [OkHttp](http://square.github.io/okhttp/) for Network Communication so you need to add them to your list of dependencies in your **build.gradle** script as well as the android.support and appcompat libraries as well as com.google.code.gson.

        dependencies {
          compile fileTree(dir: 'libs', include: ['*.jar'])
          compile 'com.android.support:support-v4:20.0.0'
          compile 'com.android.support:appcompat-v7:20.0.0'
          compile 'com.google.code.gson:gson:2.3'
          compile 'com.squareup.retrofit:retrofit:1.6.1'
          compile 'com.squareup.okhttp:okhttp-urlconnection:2.0.0'
          compile 'com.squareup.okhttp:okhttp:2.0.0'
          compile(name:'Applanga', ext:'aar')
        }

4. Make sure that the **build.gradle** script contains a configuration that marks the libs directory as a flatDir:

        repositories {
          flatDir {
            dirs 'libs'
          }
        }

5. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your App internet access, which is needed for Applanga to function. If you want to track device names you also need to add **android.permission.BLUETOOTH** in the manifest.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.BLUETOOTH" />


##Configuration
1. Download the Applanga *settingsfile* for your app from the Applanga App Overview by clicking ***[Download Settings]***. 
 
2. Add the *settingsfile* to your apps resources res/raw directory.


##Usage
Once Applanga is integrated and configured it is synchronizing your local strings with the Applanga dashboard every time you start your app or if new missing strings are found.

1. The easiest way to initialize **Applanga** in your Application is by extending your Application from **ApplangaApplication**.

		import com.applanga.android.ApplangaApplication;

        public class MyApplication extends ApplangaApplication {
			...
		}

2. To get notified on Localization Updates (e.g. to show a LoadingScreen at beginning of your App) override the **onLocalizeFinished** method.

		public class MyApplication extends ApplangaApplication {
			...
        	@Override
    		public void onLocalizeFinished(boolean success) {
				//do something on finished loacalization
    		}
    		...
    	}

3. To have a **Menu** automatically localized use the Applanga **MenuInflater**.
		
		@Override
    	public boolean onCreateOptionsMenu(Menu menu) {
        	// Inflate the menu; this adds items to the action bar if it is present.
        	Applanga.getMenuInflater().inflate(R.menu.my, menu);
        	return true;
    	}

4. If your Activity is based on ActionBarActivity you should also override **getMenuInflater** method.

		public class MyActivity extends ActionBarActivity {
			...
			@Override
   			public android.view.MenuInflater getMenuInflater() {
        		return com.applanga.android.Applanga.getMenuInflater();
    		}
    		...
    	}

5. Call the following API function to get a localized version of a string based on the current device locale.

        Applanga.getString("APPLANGA_ID");

6. To enable support for Draft Mode in your application, override the **onTouchEvent** method in your targeted activity and forward the event to Applanga.onTouchEvent. To enable Draft Mode, hold down four fingers for four seconds in this activity. A dialog appears asking you to enter a PIN, which is the first four characters of your app secret. When the right PIN is entered, the application will switch to Draft mode and quit, restart it manually. The Draft mode can be disabled in the same way as it was enabled. Please be aware that not all Android devices support multitouch with four fingers.

        public boolean onTouchEvent(MotionEvent event) {
            Applanga.onTouchEvent(event, this);
            return super.onTouchEvent(event);
        }
