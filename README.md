# Applanga SDK for Android Localization
***
*Version:* 2.0.78

*URL:* <https://www.applanga.com> 
***


## Table of Contents

  1. [Installation](#installation)
  1. [Configuration](#configuration)
  1. [Usage](#usage)
  1. [Optional settings](#optional-settings)


## Installation
***NOTE:*** *New Automatic Applanga Settings File Update Feature, see in Section 'Optional settings'.*

***NOTE:*** *The Applanga installation setup has changed! The old implementation of `applanga.gradle` should be removed, instead the Applanga Plugin will be introduced.*

***IMPORTANT***: ***Applanga SDK** and **Applanga Plugin** should always have the **same version number**!*

1. To localize your android app please add the following lines to the bottom of your Apps **build.gradle** to integrate the current version of the Applanga Plugin and Applanga localization SDK into your App.

	```gradle
	repositories {
		maven {
			url 'https://raw.github.com/applanga/sdk-android/master/maven/releases/'
		}
	}
	dependencies {
		compile 'com.applanga.android:Applanga:2.0.78'
	}
	buildscript {
		repositories {
			maven {
				url 'https://raw.github.com/applanga/sdk-android/master/maven/releases/'
			}
			jcenter()
		}
		dependencies {
			classpath  'com.applanga.android:plugin:2.0.78'
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
1. If you want to translate a android app with Applanga you need to download the *Applanga Settings File* for your app from the Applanga App Overview by clicking the ***[Prepare Release]*** button and then clicking ***[Get Settings File]***. 
2. Add the *Applanga Settings File* to your apps resources res/raw directory 


## Usage

1. **Missing Strings**
	Once Applanga is integrated and configured, it synchronizes your local strings with the Applanga dashboard every time you start your app in [Debug Mode](http://developer.android.com/tools/building/building-studio.html#RunningOnDeviceStudio) or [Draft Mode](https://applanga.com/docs#draft_on_device_testing) if new missing strings are found.

	All strings located in your project's values folder (e.g. `strings.xml`) will be uploaded. Applanga only skips the upload if they meet the following conditions (according to [Non-translatable Strings](http://tools.android.com/recent/non-translatablestrings)):
	
	Strings annotated with `translatable="false"`will be ignored, e.g.:
	```xml
	<string name="STRING_ID_IGNORED" translatable="false">This string will not be uploaded</string>
	```
	Strings inside a xml file named `donottranslate.xml` or a file with the following prefix: `donottranslate-` will not be uploaded.

2. **Callbacks**
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

3. **String Localization**

	There is not always the need to rewrite your code everywhere you want to get translated Strings. The following shows the usage with and without explicit Applanga calls:
	
	3.1 **Simple String**

	```java
	// get translated string for the current device locale
	((Activity|Resources|Fragment)this).getString(R.string.STRING_ID);
	```
	3.2 **Arguments**
	
	```java
	// get translated string with formatted arguments
	// using the default string format %s %d etc
	// @see http://developer.android.com/reference/java/util/Formatter.html
	((Activity|Resources|Fragment)this).getString(R.string.STRING_ID, "arg1", "arg2", "arg3");
	```
	3.3 **Named Arguments**
	
	```java
	// if you pass a string map you can get translated string
	// with named arguments. %{someArg} %{anotherArg} etc.
	Map<String, String> args = new Map<String, String>();
	args.put("someArg","awesome");
	args.put("anotherArg","crazy");
	Applanga.getString("STRING_ID", args);
	```

    Example:
    	
    *STRING_ID* = *"This value of the argument called someArg is %{someArg} and the value of anotherArg is **%{anotherArg}**. You can reuse arguments multiple times in your text wich is **%{someArg}**, **%{anotherArg}** and **%{someArg}.**"*	
    
    gets converted to:
    
    *"This value of the argument called someArg is awesome and the value of anotherArg is crazy. You can reuse arguments multiple times in your text wich is awesome, crazy and awesome."*    
        
	3.4 **Pluralisation**
	```java
	// get a string in the given quantity
	((Resources)res).getQuantityString(R.plurals.STRING_ID, quantity);
	
	```
	
	On the dashboard you create a **puralized ID** by appending the Pluralisation rule to your **ID** in the following format: `[zero]`, `[one]`,`[two]`,`[few]`,`[many]`, `[other]`. Plural strings also get automatically uploaded if you start the app in draft mode or with the debugger connected.
	
	So the ***zero*** pluralized ID for ***"STRING_ID"*** is ***"STRING_ID[zero]"***
	
	3.5 **String-Arrays**
	```java
	//returns a string-array
	((Resources)res).getStringArray(R.array.STRING_ID);
	```
	String-Arrays will automatically be uploaded as other strings from your string xml. The **ID**-format is the following: `STRING_ID[0]`, `STRING_ID[1]`, `STRING_ID[2]`, `STRING_ID[..]`.
	
	
	

4. **UI Localization**
	
	After you've made programmatic changes to your UI Elements you should call one of the following methods to update you UI.
	
	4.1 **Activities and Dialogs**
	
	Activity and Dialog localization is always automatically triggered when Activities layout was inflated.
	
	If you want to trigger it manually use:
	
	```java
	// localize a Activity and all its views
	Applanga.localizeContentView(activity);
	// localize a Dialog and all its views
	Applanga.localizeContentView(dialog);
	```
	
	4.2 **Views**
	
	```java
	// localize a View and all its children
	Applanga.localizeContentView(view);
	```

5. **Update Content**
 
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

	Applanga.update(groups, languages, new ApplangaCallback() {
			@Override
			public void onLocalizeFinished(boolean success) {
					//called if update is complete
			}
	});
	```

6. **Change Language**
  
  	You can change your app language at runtime using the following call:
  	
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

7. **WebViews**
	
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
	
	7.1 **Strings**
		
	The inner text and html of tags wich have a ```applanga-text="STRING_ID"``` attribute will be replaced with the translated value of ***STRING_ID***

	```html
	<div applanga-text="STRING_ID">
			***This will be replaced with the value of STRING_ID***
	</div>
	```
	
	Alternatively you can call `Applanga.getString('STRING_ID')` directly.
	
	6.2 **Arguments**
	
	You can pass arguments with the ```applanga-args``` attribute.
	By default the arguments are parsed as a comma seperated list wich then will replace fields as %{arrayIndex}. 

	```html
	<div applanga-text="STRING_ID" applanga-args="arg1,arg2,etc">
			***This will be replaced with the value of STRING_ID***
			***and formatted with arguments***
	</div>
	```
	
	Direct call : `Applanga.getString('STRING_ID', 'arg1,arg2,etc')`

	To define a different separator instead of ```,``` e.g. if your arguments contain commas use ```applanga-args-separator```.
	
	```html
	<div applanga-text="STRING_ID" 
		applanga-args="arg1;arg2;etc" 
		applanga-args-separator=";">
			***This will be replaced with the value of STRING_ID***
			***and formatted with arguments***
	</div> 
	```
	Direct call : `Applanga.getString('STRING_ID', 'arg1,arg2,etc', ';')`
		
	One Dimensional **JSON** Objects can also be used as ***Named Arguments*** if you add ```applanga-args-separator="json"```
	
	```html
	<div applanga-text="STRING_ID" 
		applanga-args="{'arg1':'value1', 'arg2':'value2', 'arg3':'etc'}"
		applanga-args-separator="json">
			***This will be replaced with the value of STRING_ID***
			***and formatted with json arguments***
	</div> 
	```

	 Direct call : `Applanga.getString('STRING_ID', "{'arg1':'value1', 'arg2':'value2', 'arg3':'etc'}", 'json')`
	
	7.3 **Pluralisation**
		
	To pluralize a html tag you can pass the ```applanga-plural-rule``` attribute with the value ```zero```, ```one```, ```two```, ```few```, ```many``` and ```other```.
	
	```html
	<div applanga-text="STRING_ID" 
		applanga-plural-rule="one">
			***This will be replaced with the pluralized value of STRING_ID***
	</div> 
	```

	Direct call : `Applanga.getPluralString('STRING_ID', 'one')` or with arguments : 	`applanga.getPluralString('STRING_ID', 'one', 'arg1;arg2;etc', ';')`
		
	You can also pluralize by quantity via `applanga-plural-quantity`

	```html
	<div applanga-text="STRING_ID" 
		applanga-plural-quantity=42>
		***This will be replaced with the pluralized value of STRING_ID***
	</div> 
	```

	Direct call : `Applanga.getQuantityString('STRING_ID', 42)` or with arguments : 	`applanga.getQuantityString('STRING_ID', 42, 'arg1;arg2;etc', ';')`	
	
	7.4 **Update Content**
	
	To trigger a content update from a WebView use javascript:

	```javascript
	Applanga.updateGroups("GroupA, GroupB", "de, en, fr", function(success){
			//called if update is complete
	});
	```

8. **Draft Mode**

	To enable support for **Draft Mode** in your application, override the ```dispatchTouchEvent``` method in your targeted activity and forward the event to Applanga.dispatchTouchEvent. To enable Draft Mode, hold down four fingers for four seconds in this activity. A dialog appears asking you to enter a key code, which is the first four characters of your app secret and can also be found in your app's main view on the dashboard. When the right key is entered, the application will switch to Draft mode and quit, restart it manually. The Draft mode can be disabled in the same way as it was enabled. Please be aware that not all Android devices support multitouch with four fingers.
	
	```java
	@Override
	public boolean dispatchTouchEvent(MotionEvent ev) {
			Applanga.dispatchTouchEvent(ev, this);
			return super.dispatchTouchEvent(ev);
	}
	```

9. **Automatic Screenshot Upload**
 	
 	The Applanga SDK offers the functionality to upload screenshots of your app, while collecting meta data such as the current language, resolution and the Applanga translated strings that are visible, 	including their positions.
 	Each screenshot will be assigned to a tag. A tag may have multiple screenshots with differing core meta data: language, app version, device, plattform, OS and resolution. 
 	
 	You can read more here: [Manage Tags](https://applanga.com/docs#manage_tags) and here: [Uploading screenshots](https://applanga.com/docs#uploading_screenshots).
 	
 	**NOTE:** To capture screenshots the app need the permission to *“Draw over other apps”* so on the first try to make a screenshot the app might redirect you to the permissions screen to enable it.
 	
 	9.1 **Make screenshots manually**
 	
 	To manually make a screenshot you first have to set your app into [draft mode](https://applanga.com/docs#draft_on_device_testing).
 	 
 	 With your app in draft mode all you have to do is to make a two finger swipe downwards.
 	This will show the screenshot menu and load a list of [tags](https://applanga.com/docs#manage_tags).
 	
 	The two finger swipe will work in activities that pass on their mouse events via ***dispatchTouchEvent*** as shown previously.
 	
 	You can now choose a tag and press *capture screenshot* to capture and upload a screenshot including all meta data for the currently visible screen and assign it to the selected tag.
 	Tags have to be created in the dashboard before they are available in the screenshot menu.
 	
 	9.2 **Display screenshot menu programmatically**
 	
 	You also have the option to display the screenshot menu programmatically, this also requires the app to be in draft mode:
	
	```java
	Applanga.setScreenShotMenuVisible(true);
	```

 	9.3 **Make screenshots programmatically**
 	
 	To create a screenshot programmatically you call the following function:

	```java
	String tag = "Mainmenu";
	List<String> applangaIDs =  new ArrayList<>();
	applangaIDs("StringID1");
	applangaIDs("StringID2");
	Applanga.captureScreenshot(tag, applangaIDs);
	```

 	The Applanga SDK tries to find all IDs on the screen but you can also pass additional IDs in the **applangaIDs** parameter.

 	9.4 **Make screenshots during UITests**
 	
 	To capture screenshots from UITests like espresso, you just have to call the above function shown in ***Make screenshots programmatically*** while executing a test with Googles UITest frameworks. This function will also work in draft mode or debug mode.
    

## Optional settings

1. **Specify default groups or languages**

	You can specify a set of default groups and languages in the manifest, which will be updated on every Applanga.update() call. These groups and languages will be added to any that are specified in the call itself, they will *always* be requested.
	The Parameter value must be a string, with a list of groups or languages separated by commata.

	*Specify default groups*

	```xml
	<application>
			...
			<meta-data android:name="ApplangaUpdateGroups" android:value="tutorial,chapter1,chapter2"/>
			...
	</application>
	```

	*Specify default languages*

	```xml
	<application>
			...
			<meta-data android:name="ApplangaUpdateLanguages" android:value="en,de-at,fr"/>
			...
	</application>
	```

2. **Optimize Applanga's performance**
	
	By setting "false" to "ApplangaRunBackgroundThread" Applanga's background thread won't start, by default it is set to "true". If you see String id's instead of their translations, please contact Applanga support to help us getting better. 
	
	```xml
	<application>
			...
			<meta-data android:name="ApplangaRunBackgroundThread" android:value="false"/>
			...
	</application>
	```

3. **Automatic Applanga Settings File Update**
	
	In case your app's user has no internet connection, new translation updates can't be fetched, so the Applanga SDK falls back to the last locally cached version. If the app was started for the first time, there are no strings locally cached yet so Applanga SDK falls back to the Applanga Settings File which contains all strings from the moment it was generated, downloaded and integrated into your app before release. 

	To minimize the manual effort of updating the Applanga Settings File, we created a task which triggers the Applanga Settings File generation if changes were made and replace the old one with the new one in your app. 

	**Be aware that the task will not fail if there is no internet connection, to be able to develop offline.** In this case, a warning is printed. To activate the Automatic Settings File Update put the following line into your `build.gradle`:

	```
	applanga.settingsFileAutoUpdate = true
	```
