# Applanga SDK for Android
***
*Version:* 2.0.61

*URL:* <https://applanga.com> 
***

## Installation
***NOTE:*** *The Applanga installation setup has changed! The old implementation of `applanga.gradle` should be removed, instead the Applanga Plugin will be introduced.*

***NOTE:*** *Android Studio 3 / Android Plugin 3 users have to set `android.enableAapt2=false` in their `gradle.properties` file and then restart the gradle daemon with `./gradlew --stop`, since aapt2 is not fully supported yet.*

***IMPORTANT***: ***Applanga SDK** and **Applanga Plugin** should always have the **same version number**!*
1. Add the following lines to the bottom of your Apps **build.gradle** to integrate the current version of the Applanga Plugin and Applanga SDK into your App.

    ```gradle
	repositories {
		maven {
			url 'https://raw.github.com/applanga/sdk-android/master/maven/releases/'
		}
	}
	dependencies {
		compile 'com.applanga.android:Applanga:2.0.61'
	}
	buildscript {
		repositories {
			maven {
				url 'https://raw.github.com/applanga/sdk-android/master/maven/releases/'
			}
			jcenter()
		}
		dependencies {
			classpath  'com.applanga.android:plugin:2.0.61'
		}
	}
	apply plugin: 'applanga'
	```

2. you should also add the latest ```appcompat``` library to your dependencies if you haven't already.

	```gradle
	dependencies {
			compile 'com.android.support:appcompat-v7:+'
	}
	```
	
3. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your App internet access, which is needed for Applanga to function.

	```xml
	<uses-permission android:name="android.permission.INTERNET" />
	```
	    
4. **Proguard:** In order to keep all SDK functionality fully available when using Proguard, please make sure that the following lines are part of your proguard configuration.

	```
	-keep class **.R$* {	
			<fields>;
	}
			
	-keepattributes JavascriptInterface
	-keepclassmembers class * {
			@android.webkit.JavascriptInterface <methods>;
	}
	```

5. The easiest way to initialize **Applanga** in your Application is by extending your `Application` class from ```ApplangaApplication```.

	```java
	import com.applanga.android.ApplangaApplication;

	public class MyApplication extends ApplangaApplication {
			...
	}
	```

	If you do not have an `Application` class you can simply add the following to the `<application .../>` section of your AndroidManifest.xml
	
	```xml
	<application
			android:name="com.applanga.android.ApplangaApplication"
			...
			>
				...
	</application>
	``` 
	***NOTE:*** *If you cannot extend ApplangaApplication, you have to call Applanga.init(), followed by Applanga.update() in your `Application` class manually (see **Update Content**).*
	

## Configuration
1. Download the *Applanga Settings File* for your app from the Applanga App Overview by clicking the ***[Prepare Release]*** button and then clicking ***[Get Settings File]***. 
 
2. Add the *Applanga Settings File* to your apps resources res/raw directory.


## Usage
Once Applanga is integrated and configured, it synchronizes your local strings with the Applanga dashboard every time you start your app in [Debug Mode](http://developer.android.com/tools/building/building-studio.html#RunningOnDeviceStudio) or [Draft Mode](https://applanga.com/#!/docs#draft_on_device_testing) if new missing strings are found.


1. **Callbacks**
	
	To get notified on Localization Updates (e.g. to show a LoadingScreen at beginning of your App) override the ```onLocalizeFinished``` method in your Application class ( assuming you extend ApplangaApplication ).
	```java
	public class MyApplication extends ApplangaApplication {
			...
			@Override
			public void onLocalizeFinished(boolean success) {
					//do something on finished loacalization
			}
			...
	}
	```
	
2. **Menus**

	To have a ```Menu``` automatically localized use the Applanga ```MenuInflater```. Please use ```Applanga.getMenuInflater(getMenuInflater())```, supplying your original MenuInflater as a parameter. Do not use the old deprecated Applanga.getMenuInflater().
	
	```java
	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
			// Inflate the menu; this adds items to the action bar if it is present.
			Applanga.getMenuInflater(getMenuInflater()).inflate(R.menu.my, menu);
			return true;
	}
	```

	You should also override the ```getMenuInflater``` method of your Activities to have menus automatically translated if possible.

	```java
	public class MyActivity extends ActionBarActivity {
			...
			@Override
			public android.view.MenuInflater getMenuInflater() {
					return com.applanga.android.Applanga.getMenuInflater(super.getMenuInflater());
			}
			...
	}
	```

3. **Preferences**

	If you use preferences you need to call on Applanga to localize the preferences after they have been set up in your preferences activity or preferences fragment:
	
	```java
	/* 
	init your preferences e.G. addPreferencesFromResource(R.xml.preferences); 
	*/
		
	Applanga.localizePreferences(getPreferenceScreen());
	```

	Keys will not be localized.

4. **Code Localization**
 
	5.1 **Strings**
	
	```java
	// get translated string for the current device locale
	Applanga.getString("APPLANGA_ID", "DEFAULT_VALUE");
	```
	
	5.2 **Arguments**
	
	```java
	// get translated string with formatted arguments
	// using the default string format %s %d etc
	// @see http://developer.android.com/reference/java/util/Formatter.html
	Applanga.getString("APPLANGA_ID", "DEFAULT_VALUE", "arg1", "arg2", "etc.");
	```

	5.3 **Named Arguments**
	
	```java
	// if you pass a string map you can get translated string
	// with named arguments. %{someArg} %{anotherArg} etc.
	Map<String, String> args = new Map<String, String>();
	args.put("someArg","awesome");
	args.put("anotherArg","crazy");
	Applanga.getString("APPLANGA_ID", args);
	```

    Example:
    	
    *APPLANGA_ID* = *"This value of the argument called someArg is %{someArg} and the value of anotherArg is **%{anotherArg}**. You can reuse arguments multiple times in your text wich is **%{someArg}**, **%{anotherArg}** and **%{someArg}.**"*	
    
    gets converted to:
    
    *"This value of the argument called someArg is awesome and the value of anotherArg is crazy. You can reuse arguments multiple times in your text wich is awesome, crazy and awesome."*    
        
	5.4 **Pluralisation**
	
	```java
	// get translated string in given pluralisation rule (one)
	Applanga.getString("APPLANGA_ID", Applanga.PluralRule.One);
	```

	Available pluralisation rules:
	
	```java
	Zero,
	One,
	Two,
	Few,
	Many,
	Other
	```
	
	you can also specify a quantity and Applanga will pick the best pluralisation rule based on: [http://www.unicode.org/...plurals.html	](http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html)
	
	```java
	// get a string in the given quantity
	Applanga.getQuantityString("APPLANGA_ID", "DEFAULT_VALUE", quantity);
	```
	
	In the dashboard you create a **puralized ID** by appending the Pluralisation rule to your **ID** in the following format: `[zero]`, `[one]`,`[two]`,`[few]`,`[many]`, `[other]`.
	
	So the ***zero*** pluralized ID for ***"APPLANGA_ID"*** is ***"APPLANGA_ID[zero]"***
               
5. **UI Localization**
	
	After you've made programmatic changes to your UI Elements you should call one of the following methods to update you UI.
	
	6.1 **Activities**
	
	Activity localization is always automatically triggered when a Activity is loaded or resumed. (onResume)
	
	If you want to trigger it manually use:
	
	```java
	// localize a Activity and all its Views
	Applanga.localizeActivity(activity);
	```
	
	6.2 **Views**
	
	```java
	// localize a View and all its children
	Applanga.localizeView(view);
	```
	
	6.3 **Menus**
	
	```java
	// localize a Menu
	Applanga.localizeMenu(menu);
	```
	
6. **Update Content**
 
	To trigger an update call:
	
	```java
	Applanga.update(new ApplangaCallback() {
			@Override
			public void onLocalizeFinished(boolean success) {
					//called if update is complete    
			}
	});
	```

    This will request the baselanguage and the long and short versions of the device's current language. If you are using groups, be aware that this will only update the **main** group.

	To trigger an update for a specific set of groups and languages call:

	```java
	List<String> groups = new ArrayList<>();
	groups.add("GroupA");
	groups.add("GroupB");
	List<String> languages = new ArrayList<>();
	languages.add("en");
	languages.add("de");
	languages.add("fr");

	Applanga.updateGroups(groups, languages, new ApplangaCallback() {
			@Override
			public void onLocalizeFinished(boolean success) {
					//called if update is complete
			}
	});
	```

7. **Change Language**
  
  	You can change your app's language at runtime using the following call:
  	
	```java
	boolean success = Applanga.setLanguage(language);
	```
	
  	 *language* must be the iso string of a language that has been added in 	the dashboard. 
  	The return value will be *true* if the language could be set, or if it already was the 	current language, otherwise it will be *false*. After a successful call you should  	recreate the current activity, for the changes to take effect.
  	The set language will be saved, to reset to the 	device language call:
	
	```java
  	Applanga.setLanguage(null); 
	```
  	
  	For the app to reset to the device language it needs to be restarted.
  	
  	The *language* parameter is expected in the format **[language]-[region]** or 	**[language]_[region]** with region being optional. Examples: "fr_CA", "en-us", "de". 
  	
  	If you have problems switching to a specific language you can update your settings file 	or specifically request that language within an update content call (see **8. Update Content**). You can also 	specify the language as a default language to have it requested on each update call (see **Optional settings**).
  
8. **WebViews**
	
	Applanga can also translate content in your WebViews if they have JavaScript enabled.
	
	```
	WebView myWebView = (WebView) findViewById(R.id.webview);
	myWebView.getSettings().setJavaScriptEnabled(true);
	myWebView.loadUrl("html_file_path");
	```
	
	To tell Applanga to translate your webviews you need to set the following in your Manifest:
	
	```
	<meta-data android:name="ApplangaTranslateWebViews" android:value="true" />
	```
	
	To initalize Applanga for your webcontent you need to initialize Applanga from JavaScript:
	
	```html
	<script type="text/javascript">
			window.initApplanga = function() {
					if(typeof window.ApplangaNative !== 'undefined'){ window.ApplangaNative.loadScript();
					} else { setTimeout(window.initApplanga, 180); }
			}; window.initApplanga();
	</script>	
	```
	
	8.1 **Strings**
		
	The inner text and html of tags wich have a ```applanga-text="APPLANGA_ID"``` attribute will be replaced with the translated value of ***APPLANGA_ID***

	```html
	<div applanga-text="APPLANGA_ID">
			***This will be replaced with the value of APPLANGA_ID***
	</div>
	```
	
	Alternatively you can call `Applanga.getString('APPLANGA_ID')` directly.
	
	8.2 **Arguments**
	
	You can pass arguments with the ```applanga-args``` attribute.
	By default the arguments are parsed as a comma seperated list wich then will replace fields as %{arrayIndex}. 

	```html
	<div applanga-text="APPLANGA_ID" applanga-args="arg1,arg2,etc">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with arguments***
	</div>
	```
	
	Direct call : `Applanga.getString('APPLANGA_ID', 'arg1,arg2,etc')`

	To define a different separator instead of ```,``` e.g. if your arguments contain commas use ```applanga-args-separator```.
	
	```html
	<div applanga-text="APPLANGA_ID" 
		applanga-args="arg1;arg2;etc" 
		applanga-args-separator=";">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with arguments***
	</div> 
	```
	Direct call : `Applanga.getString('APPLANGA_ID', 'arg1,arg2,etc', ';')`
		
	One Dimensional **JSON** Objects can also be used as ***Named Arguments*** if you add ```applanga-args-separator="json"```
	
	```html
	<div applanga-text="APPLANGA_ID" 
		applanga-args="{'arg1':'value1', 'arg2':'value2', 'arg3':'etc'}"
		applanga-args-separator="json">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with json arguments***
	</div> 
	```

	 Direct call : `Applanga.getString('APPLANGA_ID', "{'arg1':'value1', 'arg2':'value2', 'arg3':'etc'}", 'json')`
	
	8.3 **Pluralisation**
		
	To pluralize a html tag you can pass the ```applanga-plural-rule``` attribute with the value ```zero```, ```one```, ```two```, ```few```, ```many``` and ```other```.
	
	```html
	<div applanga-text="APPLANGA_ID" 
		applanga-plural-rule="one">
			***This will be replaced with the pluralized value of APPLANGA_ID***
	</div> 
	```

	Direct call : `Applanga.getPluralString('APPLANGA_ID', 'one')` or with arguments : 	`applanga.getPluralString('APPLANGA_ID', 'one', 'arg1;arg2;etc', ';')`
		
	You can also pluralize by quantity via `applanga-plural-quantity`

	```html
	<div applanga-text="APPLANGA_ID" 
		applanga-plural-quantity=42>
		***This will be replaced with the pluralized value of APPLANGA_ID***
	</div> 
	```

	Direct call : `Applanga.getQuantityString('APPLANGA_ID', 42)` or with arguments : 	`applanga.getQuantityString('APPLANGA_ID', 42, 'arg1;arg2;etc', ';')`	
	
	8.4 **Update Content**
	
	To trigger a content update from a WebView use javascript:

	```javascript
	Applanga.updateGroups("GroupA, GroupB", "de, en, fr", function(success){
			//called if update is complete
	});
	```

9. **Draft Mode**

	To enable support for **Draft Mode** in your application, override the ```dispatchTouchEvent``` method in your targeted activity and forward the event to Applanga.dispatchTouchEvent. To enable Draft Mode, hold down four fingers for four seconds in this activity. A dialog appears asking you to enter a key code, which is the first four characters of your app secret and can also be found in your app's main view on the dashboard. When the right key is entered, the application will switch to Draft mode and quit, restart it manually. The Draft mode can be disabled in the same way as it was enabled. Please be aware that not all Android devices support multitouch with four fingers.
	
	```java
	@Override
	public boolean dispatchTouchEvent(MotionEvent ev) {
			Applanga.dispatchTouchEvent(ev, this);
			return super.dispatchTouchEvent(ev);
	}
	```

10. **Automatic Screenshot Upload**
 	
 	The Applanga SDK offers the functionality to upload screenshots of your app, while collecting meta data such as the current language, resolution and the Applanga translated strings that are visible, 	including their positions.
 	Each screenshot will be assigned to a tag. A tag may have multiple screenshots with differing core meta data: language, app version, device, plattform, OS and resolution. 
 	
 	You can read more here: [Manage Tags](https://applanga.com/#!/docs#manage_tags) and here: [Uploading screenshots](https://applanga.com/#!/docs#uploading_screenshots).
 	
 	**NOTE:** To capture screenshots the app need the permission to *“Draw over other apps”* so on the first try to make a screenshot the app might redirect you to the permissions screen to enable it.
 	
 	11.1 **Make screenshots manually**
 	
 	To manually make a screenshot you first have to set your app into [draft mode](https://applanga.com/#!/docs#draft_on_device_testing).
 	 
 	 With your app in draft mode all you have to do is to make a two finger swipe downwards.
 	This will show the screenshot menu and load a list of [tags](https://applanga.com/#!/docs#manage_tags). 
 	
 	The two finger swipe will work in activities that pass on their mouse events via ***dispatchTouchEvent*** as shown previously.
 	
 	You can now choose a tag and press *capture screenshot* to capture and upload a screenshot including all meta data for the currently visible screen and assign it to the selected tag.
 	Tags have to be created in the dashboard before they are available in the screenshot menu.
 	
 	11.2 **Display screenshot menu programmatically**
 	
 	You also have the option to display the screenshot menu programmatically, this also requires the app to be in draft mode:
	
	```java
	Applanga.setScreenShotMenuVisible(true);
	```

 	11.3 **Make screenshots programmatically**
 	
 	To create a screenshot programmatically you call the following function:

	```java
	String tag = "Mainmenu";
	List<String> applangaIDs =  new ArrayList<>();
	applangaIDs("StringID1");
	applangaIDs("StringID2");
	Applanga.captureScreenshot(tag, applangaIDs);
	```

 	The Applanga SDK tries to find all IDs on the screen but you can also pass additional IDs in the **applangaIDs** parameter.

 	11.4 **Make screenshots during UITests**
 	
 	To capture screenshots from UITests like espresso, you just have to call the above function shown in ***Make screenshots programmatically*** while executing a test with Googles UITest frameworks. This function will also work in draft mode or debug mode.
    

## Optional settings

You can specify a set of default groups and languages in the manifest, which will be updated on every Applanga.update() or Applanga.updateGroups() call. These groups and languages will be added to any that are specified in the call itself, they will *always* be requested.
	The Parameter value must be a string, with a list of groups or languages separated by commata.

1. **Specify default groups**

	```xml
	<application>
			...
			<meta-data android:name="ApplangaUpdateGroups" android:value="tutorial,chapter1,chapter2"/>
			...
	</application>
	```

2. **Specify default languages**

	```xml
	<application>
			...
			<meta-data android:name="ApplangaUpdateLanguages" android:value="en,de-at,fr"/>
			...
	</application>
	```
	
