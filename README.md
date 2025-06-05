# Applanga SDK for Android Localization
***
*Version:* 4.0.216

*Website:* <https://www.applanga.com>

*Changelog:* <https://www.applanga.com/changelog/android>
***


## Table of Contents

  1. [Installation](#installation)
  2. [Configuration](#configuration)
  3. [Usage](#usage)
  4. [Optional settings](#optional-settings)
  5. [Branching](#branching)

***

## Applanga 4.0 Major Changes 

### Plugin

* ***The Applanga plugin is only compatible with projects that use the Android Gradle Plugin 7+.***
If you still depend on Android Gradle Plugin 4+, please use Applanga version 3+. We rewrote big parts of the plugin to align with the newest AGP APIs, which makes the build a lot faster and more stable.

* We changed the package name of the plugin, you also can add it now via the `plugins{}` block in your `build.gradle`.

### SDK

* The Applanga SDK will automatically initialize as soon as the first activity starts. This overcomes a lot of issues we had in the past regarding wrong initialization, especially connected to deep link scenarios.

* You can still manually initialize Applanga, but this is only needed if you want to use Applanga before an activity has started. For example, if you want to do an `Applanga.update()` on startup before an activity has started.

* `Applanga.wrap()` isn't needed anymore, it's done automatically by the plugin.

* We deprecated `Applanga.getString` methods as we cover all usual `getString()` calls without further configurations and we have seen a lot of confusion about the existence of `Applanga.getString` calls.
 
* We removed all `Applanga.getQuantityString` calls as all native `getQuantityString` calls are covered by Applanga.

* `Applanga.setDraftModelEnabled` has a typo and was renamed to `Applanga.setDraftModeEnabled`.

* Applanga doesn't require `jitpack.io` as a repository in your build.gradle files anymore. If you still use `jitpack.io` for any reason, we recommend to move `mavenCentral()` above `jitpack.io` as it's more reliable and order matters.

***

## Installation

The Applanga SDK comes with an Applanga Gradle Plugin. Both work closely together so make sure they have the same version number. The responsibility of the plugin is to proxy all your `getString` or `setText` calls to the Applanga SDK so that you receive the latest over-the-air updates.

### SDK
Add the Applanga maven repository and the Applanga dependency to your `app/build.gradle` file.
```gradle
// $projectDir/app/build.gradle
repositories {
    maven { url 'https://maven.applanga.com/'}
}
dependencies {
    implementation 'com.applanga.android:Applanga:4.0.216'
}
```

### Plugin
As shortly described above, the plugin is responsible for proxying all relevant `getString` calls through the Applanga SDK.
It also collects data from your resource files, which is needed for accurate translations of specific views.
This metadata data is stored in `applanga_meta.xml` in your `assets/` folder. We recommend that you ignore this file and that you don't commit it to your repository.
```gradle
// .gitignore
...
*applanga_meta.xml
```

There are two different ways how to apply this plugin:

#### 1. Add the plugin via plugins DSL

```gradle
// $projectDir/app/build.gradle
plugins {
    ...
    id 'com.applanga.gradle' version '4.0.216'
}
```
Insert our Applanga maven repository to the `pluginManagement.repositories` section.
```gradle
// $projectDir/settings.gradle
pluginManagement {
    repositories {
        ...
        google()
        mavenCentral()
        maven { url 'https://maven.applanga.com/'}
    }
}
```

#### 2. Add the plugin the old way
Apply the plugin and add the Applanga maven repository to the `buildscript.repositories`.
```gradle
// $projectDir/app/build.gradle
apply plugin: 'com.applanga.gradle'
...
buildscript {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://maven.applanga.com/' }
    }
    dependencies {
        classpath  'com.applanga.gradle:plugin:4.0.216'
    }
}
```
_***IMPORTANT***:_ **Applanga SDK** and **Applanga Plugin** should always have the **same version number**!

### Proguard
In order to keep all SDK functionality fully available when using Proguard, please make sure that the following lines are part of your Proguard configuration:

    ```
    -keep class **.R$* {
            <fields>;
    }

    -keepattributes JavascriptInterface
    -keepclassmembers class * {
            @android.webkit.JavascriptInterface <methods>;
    }
    ```
    
## Initialization

#### 1. **Auto init**

The easiest way to initialize **Applanga** in your Application is by extending your `Application` class from ```ApplangaApplication```.


```java
    
import com.applanga.android.ApplangaApplication;

public class MyApplication extends ApplangaApplication {
        ...
}
    
```


If you don't have an `Application` class, you can simply add the following to the `<application .../>` section of your AndroidSettings File.xml:

```xml
<application
        android:name="com.applanga.android.ApplangaApplication"
        ...
        >
            ...
</application>
```

***NOTE:*** *If you can't extend `ApplangaApplication`, you have to call `Applanga.init()`, followed by `Applanga.update()` in your `Application` class manually (see **Update Content**).*

#### 2. **Manual Init**

You can also choose to manually init the Applanga SDK like so:

```java
//called from an activity
Applanga.update(new ApplangaCallback() {
    @Override
    public void onLocalizeFinished(boolean b) {
        //continue loading your app
    }
});
        
```
The advantage of this method is that you can stop the app from loading any texts until the Applanga SDK has pulled the latest strings from the dashboard.

In [this example app](https://github.com/applanga/AndroidBasicUseCaseDemo), you can see that we have a loading screen activity that inits and updates Applanga, and then loads the first Activity in the app once the update is complete.

## Configuration
1. If you want to translate an Android app with Applanga, you need to download the Applanga Settings File for your app from the Applanga project dashboard by clicking the ***[Prepare Release]*** button and then clicking ***[Get Settings File]***.
2. Add the Applanga Settings File to your app's resources res/raw directory.


## Usage

1. **Missing Strings**
    Once Applanga is integrated and configured, it synchronizes your local strings with the Applanga dashboard every time you start your app in [Debug Mode](http://developer.android.com/tools/building/building-studio.html#RunningOnDeviceStudio) or [Draft Mode](https://www.applanga.com/docs/applanga-mobile-sdks/draft_on-device-testing) if new missing strings are found.

    All strings located in your project's values folder (e.g. `strings.xml`) will be uploaded. Applanga only skips the upload of strings that meet the following conditions (according to [Non-translatable Strings](http://tools.android.com/recent/non-translatablestrings)):

    Strings annotated with `translatable="false"` will be ignored, e.g.:
    ```xml
    <string name="STRING_ID_IGNORED" translatable="false">This string won't be uploaded</string>
    ```
    Strings inside an XML file named `donottranslate.xml` or a file with the `donottranslate-` prefix won't be uploaded.
2. **Callbacks**
    To get notified on localization updates (e.g. to show a LoadingScreen at beginning of your App) override the ```onLocalizeFinished``` method in your Application class (assuming you extend ApplangaApplication).
    ```java
    public class MyApplication extends ApplangaApplication {
            ...
            @Override
            public void onLocalizeFinished(boolean success) {
                    //do something on finished localization
            }
            ...
    }
    ```
3. **String Localization**

    You don't need to rewrite your code everywhere you want to get translated strings. The following shows the usage with and without explicit Applanga calls:

    3.1 **Simple String**

    If the string exists locally in your project: 
    ```java
    // get translated string for the current device locale
    ((Activity|Resources|Fragment)this).getString(R.string.STRING_KEY);
    ```
    For dynamic keys retrieved from the backend or other sources outside of your resource files, you can call `Applanga.getTranslation`. However, dynamic keys won't be created automatically in the Applanga project. You need to make sure the key exists in the project or else the method will return `null`. 
    ```java
    Applanga.getTranslation("STRING_KEY");
    ```
    3.2 **Arguments**

    ```java
    // get translated string with formatted arguments
    // using the default string format %s %d etc
    // @see http://developer.android.com/reference/java/util/Formatter.html
    ((Activity|Resources|Fragment)this).getString(R.string.STRING_KEY, "arg1", "arg2", "arg3");
    ```

    The dynamic key equivalent:
    ```java
    Applanga.getTranslation("STRING_KEY", "arg1", "arg2", "arg3");
    ```
    3.3 **Named Arguments DEPRECATED!**

    We deprecated named Arguments and it will be removed on the next major release.
    If you want to align your placeholder logic on Android and iOS, please read the **Convert Placeholders section** in this article.

    ```java
    // DEPRECATED!
    
    // if you pass a string map you can get translated string
    // with named arguments. %{someArg} %{anotherArg} etc.
    Map<String, String> args = new Map<String, String>();
    args.put("someArg","awesome");
    args.put("anotherArg","crazy");
    Applanga.getString("STRING_KEY", args);
    ```

    Example:

    *STRING_ID* = *"This value of the argument called someArg is %{someArg} and the value of anotherArg is **%{anotherArg}**. You can reuse arguments multiple times in your text which is **%{someArg}**, **%{anotherArg}** and **%{someArg}.**"*

    gets converted to:

    *"This value of the argument called someArg is awesome and the value of anotherArg is crazy. You can reuse arguments multiple times in your text which is awesome_, crazy_, _ awesome._"*

    3.4 **Pluralisation**
    
    ```java
    // get a string in the given quantity
    ((Resources)res).getQuantityString(R.plurals.STRING_KEY, quantity);
    ```

    On the dashboard, you can create a **pluralized ID** by appending the pluralisation rule to your **ID** in the following format: `[zero]`, `[one]`,`[two]`,`[few]`,`[many]`, `[other]`. Plural strings also get automatically uploaded if you start the app in Draft Mode or with the debugger connected.

    So the ***zero*** pluralized ID for ***"STRING_KEY"*** is ***"STRING_KEY[zero]"***

    You also can get quantity strings dynamically:
    ```java
    Applanga.getQuantityTranslation("STRING_KEY", quantity);
    ```
    Dynamic quantity translations have the limitation than `Applanga.getTranslation`: if the passed string key doesn't exist on the dashboard, it will return `null`.
    It doesn't automatically create the string key on the dashboard if it doesn't exist yet.

    3.5 **String-Arrays**
    ```java
    //returns a string-array
    ((Resources)res).getStringArray(R.array.STRING_KEY);
    ```
    String-Arrays will automatically be uploaded like the other strings from your strings.xml file. The **ID** format is the following: `STRING_KEY[0]`, `STRING_KEY[1]`, `STRING_KEY[2]`, `STRING_KEY[..]`.

    You also can get string arrays dynamically:
    ```java
    Applanga.getTranslationArray("STRING_KEY");
    ```
    Dynamic translation arrays have the limitations as described for `getTranslation`: if the passed string key doesn't exist on the dashboard, it returns `null`. That is, it doesn't automatically create the string key on the dashboard if it doesn't exist yet.

    3.6 **Styled Strings**
    
    We don't support HTML tags in XML files yet. To make the upload of styled strings from your `strings.xml` work, you have to escape the HTML tags.

    ```xml
    <string name="styled_string">Hello, &lt;b>I am bold&lt;/b>.</string>
    ```

    If you don't escape your styled strings in your strings.xml file, then the HTML tags are stripped out with the missing string upload.
    However if you translate your strings on the Applanga dashboard, you don't have to escape the HTML tags.

    To get the styled string as a SpannableString, use `Html.fromHtml()` as explained in the official [Android documentation](https://developer.android.com/guide/topics/resources/string-resource#FormattingAndStyling).

    ```java
    textView.setText(Html.fromHtml(getString(R.string.styled_string), Html.FROM_HTML_MODE_LEGACY));
    ```

4. **Preference Localization**

    With the Applanga plugin, Preference Localization is mostly automated as of Applanga version 4.0.216 However, if you want to enable Preference localization, every `PreferenceItem` (including `PreferenceCategory`) must have a key. After a Preference has been localized, there will be a log output stating: "localize Preferences!".

	As an example, a working preference XML would look like this:
	
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
	<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:tools="http://schemas.android.com/tools">
    	<PreferenceCategory
    	android:key="example_category_key"
    	android:title="@string/example_category_key_title">
    		<Preference
      		android:key="example_pref_key"
      		android:title="@string/example_pref_title"/>
  		</PreferenceCategory>
  	</PreferenceScreen>
    ```

    We added linter warnings for all preference items without a key.
    To disable those warnings you can add `tools:ignore="ApplangaPreferenceKeyMissing"` to your item or to the parent to disable the linting for all child items as well. E.g.:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
	<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:tools="http://schemas.android.com/tools"
                  tools:ignore="ApplangaPreferenceKeyMissing">
    	<PreferenceCategory
    	android:title="@string/example_category_key_title">
    		<Preference
      		android:title="@string/example_pref_title"/>
  		</PreferenceCategory>
  	</PreferenceScreen>
    ```


5. **Menu Localization**

    Menu localization doesn't require any additional code to work, but every menu item **must have an ID**.
    The logic in this case is similar to the Preference Localization in the section above.
    We also added lint checks, you can disable them with `tools:ignore="ApplangaMenuIdMissing"`.

    Below you can find a small example with one item without an ID and one with an ID with disabled linting:
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:tools="http://schemas.android.com/tools"
        xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:title="@string/main_menu_no_id_title"
            tools:ignore="ApplangaMenuIdMissing" />

        <item android:id="@+id/main_menu_change_language"
            android:title="@string/main_menu_change_language"/>
    </menu>
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

    Applanga.update(groups, languages, new ApplangaCallback() {
            @Override
            public void onLocalizeFinished(boolean success) {
                    //called if update is complete
            }
    });
    ```

7. **Change Language**

    ### Recommended: Per-app language preferences

    Android 13 introduced the [per-app language preference](https://developer.android.com/guide/topics/resources/app-languages).
    If you implement your own language picker within your app, make sure to call `Applanga.update(groups, languages, callback)` with the new language. Otherwise the language will change but there will be no over-the-air updates until the next `update` call.
    Additionally, make sure that your Settings File is up to date. This way, you can switch languages while the app is running and get the most recent strings from your Settings File even if the `Applanga.update` doesn’t work due to network connection issues or if it simply didn't happen.  

    The AppCompat library also works together with the Applanga SDK. To switch languages, you can do the following: `AppCompatDelegate.setApplicationLocales(appLocale);` 

    **Important:** the legacy methods `Applanga.setLanguage()` or `Applanga.setLanguageAndUpdate()` store the changed language to your device.
    To delete its settings you need to call `Applanga.setLanguage(null)` once.

    ### Legacy: Applanga.setLanguage(language)

    **You should consider using the per-app language preferences as described above.**

      You can change your app language at runtime using the following call:

    ```java
    // this changes the language but your initial update call may not cover this language
    // so it's good practice to do an Applanga.update() afterwards or use the `setLanguageAndUpdate` method
    boolean success = Applanga.setLanguage(language); 
    // - or - 
    // this sets the language and directly does an update for the language before returning here
    Applanga.setLanguageAndUpdate(language, applangaCallback);
    ```

      `language` must be the ISO language code of the language that has been added to the project dashboard.
      
      The return value will be `true` if the language has been set or if it already is the current language. Otherwise, the return value will be `false`.

      After a successful call, you should recreate the current activity for the changes to take effect.
      The set language will be saved.
      
      To reset the app to the device language, call the following method and restart the app:

    ```java
      Applanga.setLanguage(null);
    ```

    The *language* parameter is expected in the format **[language]-[region]** or **language****_region** (the region is optional). Examples: "fr_CA", "en-us", "de".

    If you have problems switching to a specific language, you can update your Settings File or specifically request that language within an update content call (see **8. Update Content**). You can also define a default language to have it requested on each update call (see **Optional settings**).

8. **WebViews**

    Applanga can also translate content in your WebViews. You have to enable JavaScript and pass your WebView to Applanga's `attachWebView(WebView webView)`:

    ```
    WebView myWebView = (WebView) findViewById(R.id.webview);
    myWebView.getSettings().setJavaScriptEnabled(true);
    myWebView.loadUrl("html_file_path");
    Applanga.attachWebView(myWebView);
    ```

    You also need to set the following in your Manifest:

    ```
    <meta-data android:name="ApplangaTranslateWebViews" android:value="true" />
    ```

    To initialize Applanga for your web content, you need to initialize Applanga from JavaScript:

    ```html
    <script type="text/javascript">
            window.initApplanga = function() {
                    if(typeof window.ApplangaNative !== 'undefined'){ window.ApplangaNative.loadScript();
                    } else { setTimeout(window.initApplanga, 180); }
            }; window.initApplanga();
    </script>
    ```

    8.1 **Strings**

    The inner text and HTML of tags that have a `applanga-text=`"STRING_ID"``` attribute will be replaced with the translated value of ***STRING_ID***

    ```html
    <div applanga-text="STRING_ID">
            ***This will be replaced with the value of STRING_ID***
    </div>
    ```

    Alternatively, you can call `Applanga.getString('STRING_ID')` directly.

    8.2 **Arguments**

    You can pass arguments with the ```applanga-args``` attribute.
    By default, the arguments are parsed as a comma-separated list that will then replace fields as %{arrayIndex}.

    ```html
    <div applanga-text="STRING_ID" applanga-args="arg1,arg2,etc">
            ***This will be replaced with the value of STRING_ID***
            ***and formatted with arguments***
    </div>
    ```

    Direct call : `Applanga.getString('STRING_ID', 'arg1,arg2,etc')`

    To define a different separator instead of a comma (e.g. in case your arguments contain commas), use `applanga-args-separator`.

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

    8.3 **Pluralisation**

    To pluralize an HTML tag you can pass the ```applanga-plural-rule``` attribute with the values `zero`, ```one```, ```two```, ```few```, ```many```, and ```other```.

    ```html
    <div applanga-text="STRING_ID"
        applanga-plural-rule="one">
            ***This will be replaced with the pluralized value of STRING_ID***
    </div>
    ```

    Direct call : `Applanga.getPluralString('STRING_ID', 'one')` or with arguments:     `applanga.getPluralString('STRING_ID', 'one', 'arg1;arg2;etc', ';')`

    You can also pluralize by quantity via `applanga-plural-quantity`

    ```html
    <div applanga-text="STRING_ID"
        applanga-plural-quantity=42>
        ***This will be replaced with the pluralized value of STRING_ID***
    </div>
    ```

    Direct call : `Applanga.getQuantityString('STRING_ID', 42)` or with arguments :     `applanga.getQuantityString('STRING_ID', 42, 'arg1;arg2;etc', ';')`

    8.4 **Update Content**

    To trigger a content update from a WebView use javascript:

    ```javascript
    Applanga.updateGroups("GroupA, GroupB", "de, en, fr", function(success){
            //called if update is complete
    });
    ```

9. **Draft Mode**

    To enable support for **Draft Mode** in your application, override the ```dispatchTouchEvent``` method in your targeted activity and forward the event to Applanga.dispatchTouchEvent. To enable Draft Mode, hold down six fingers for six seconds in this activity. A dialog will appear asking you to enter a key code. The code is the first four characters of your app secret and can be found in the Applanga project dashboard. If the key is correct, the application will switch to Draft Mode and quit, so you will need to restart it manually. The Draft Mode can be disabled in the same way it was enabled. Please be aware that not all Android devices support multitouch with six fingers.

    ```java
    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
            Applanga.dispatchTouchEvent(ev, this);
            return super.dispatchTouchEvent(ev);
    }
    ```

    The Draft Mode menu also requires the alert permission to work. Please add it to your Manifest like so:

    ```xml
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
    ```


10. **Automatic Screenshot Upload**

     The Applanga SDK offers the functionality to upload screenshots of your app while collecting meta-data such as the current language, resolution and the Applanga translated strings that are visible, including their positions.
     Each screenshot will be assigned to a tag. A tag may have multiple screenshots with differing core meta-data (e.g. language, app version, device, platform, OS, and resolution).

     You can read more on our [Tags](https://www.applanga.com/docs/translation-management-dashboard/applanga-tags) and [Uploading screenshots](https://www.applanga.com/docs/applanga-screenshots/uploading-screenshots) articles.

     **NOTE:** to capture screenshots the app needs permission to _“_Draw over other apps”. When you first try to make a screenshot, the app might redirect you to the permissions screen to enable it.

     10.1 **Make screenshots manually**

     To manually make a screenshot you first have to set your app into [Draft Mode](https://www.applanga.com/docs/applanga-mobile-sdks/draft_on-device-testing).

     With your app in Draft Mode, all you have to do is to make a two-finger swipe downwards.
     This will show the screenshot menu and load a list of [tags](https://www.applanga.com/docs/translation-management-dashboard/applanga-tags).

     As shown previously, the two-finger swipe will work in activities that pass on their mouse events via ***dispatchTouchEvent***.

     You can now choose a tag and press *capture screenshot* to capture and upload a screenshot (including all meta-data for the currently visible screen) and assign it to the selected tag.
     Tags have to be created in the dashboard before they are available in the screenshot menu.

     10.2 **Display screenshot menu programmatically**

     You also have the option to display the screenshot menu programmatically, this also requires the app to be in Draft Mode:

    ```java
    Applanga.setScreenShotMenuVisible(true);
    ```

     10.3 **Make screenshots programmatically**

     To create a screenshot programmatically, call the following function:

    ```java
    String tag = "Mainmenu";
    List<String> applangaIDs =  new ArrayList<>();
    applangaIDs("StringID1");
    applangaIDs("StringID2");
    Applanga.captureScreenshot(tag, applangaIDs);
    ```

     The Applanga SDK will try to find all IDs on the screen, but you can also pass additional IDs in the **applangaIDs** parameter.

     10.4 **Make screenshots during UITests**

     To capture screenshots from UITests like Espresso, you just have to call the above function shown in ***Make screenshots programmatically*** while executing a test with Googles UITest frameworks. This function will also work in Draft Mode or debug mode.

    The Applanga SDK should automatically find the tags of all the text strings on the screen. However, it’s possible that a text string isn't tagged, or that the SDK can't find the correct tag. There are different reasons for it, the most common reason is strings set during runtime. The best method to get all current strings on the screen correctly tagged is to use the `showIdMode`.
    
    The `showIdMode` shows all string IDs instead of the translations. When this mode is enabled, the SDK can collect all string IDs on the screen. It's more reliable than OCR and it works even with localizations set at runtime. 

    Usage:

    ```java
    Applanga.setShowIdModeEnabled(true);
    ```

    Note:
    if `setIdModeEnabled` is set after showing a specific screen, you have to recreate the activity or set the flag before initializing your screen.


     10.6 **OCR screenshots**
     
     The Applanga SDK automatically finds the tags of all texts on screen, but if for some reason a text isn't tagged or the SDK can't find the correct tag, you can take a screenshot programmatically using the `enableOcr` parameter like so:
	```java
	Applanga.captureScreenshot(tag, applangaIDs, true);
	```
	
	Please note that in most cases, enabling OCR isn't necessary and will slow down the processing of screenshots for the dashboard, so please only use it if needed. If you need more information or have any questions, feel free to reach out to [support@applanga.com](support@applanga.com) or use the support chat in the bottom right corner of the website.

11. **Multi-project setup**

	The multi-project setup is the same as described in the *Installation* section.
    You only have to add the SDK and the Applanga plugin to your module if the module contains translatable resources (string.xml or layout files), if it inflates layouts from other modules, or if you simply want to use the Applanga SDK for any reason. Otherwise, modules without the Applanga SDK and plugin remain untranslated. 

12. **Custom ViewPump Initialization**

	If you are already using [ViewPump](https://github.com/InflationX/ViewPump) in your app (i.e. [Calligraphy](https://github.com/InflationX/Calligraphy)) it is advised to initialize [ViewPump](https://github.com/InflationX/ViewPump) before `Applanga.init(...)` because that way all interceptors will stay active. If you need to initialize it at a later stage, please make sure to cache and re-add the existing interceptors as shown in the sample below.
	
    ```java
    //cache existing (Applanga) interceptors
    List<Interceptor> interceptors = ViewPump.get().interceptors();

    //create a new ViewPump.Builder
    ViewPump.Builder builder = ViewPump.builder().addInterceptor(
    								new CalligraphyInterceptor(
                						new CalligraphyConfig.Builder()
                        					.setDefaultFontPath("fonts/Roboto-ThinItalic.ttf")
                        					.setFontAttrId(R.attr.fontPath)
                        					.build()));

    //re-add cached (Applanga) interceptors
    for(int i = 0; i  < interceptors.size(); i++) {
	    builder.addInterceptor(interceptors.get(i));
    }

    //re-initialize ViewPump
    ViewPump.init(builder.build());
    ```
    
13. **Unit Testing and Robolectric**

    When running Unit tests, Applanga only returns local resources.
    The Applanga plugin replaces all `getString()` calls with the Applanga SDK.
    Depending on your tests, make sure that the Applanga SDK is initialized with a (mocked) Context, to be able to access your local resources.
    In most cases the Applanga SDK can initialize itself, so it may not be necessary to do so.
    The following log output should appear: `Unit tests are running! Applanga returns local strings only.`. If you use Robolectric, you should see: `Robolectric in use.`.

    If you don't see the logs or encounter any other issues please contact us at [support@applanga.com](support@applanga.com) or use the support chat in the bottom right corner of the website

14. **Get available languages**

  	It is possible to get the list of languages that are currently part of a project:
  	
	```java
	// java
    List<String> languages = Applanga.getAvailableLanguages();
	```
	```kotlin
	// kotlin
	val languages = Applanga.getAvailableLanguages()
	```
	
	The result will contain the ISO language code of each language that has been added to the project.
	These values are synced during the `Applanaga.update()` response. To retrieve any changes to the available languages on the dashboard, an `Applanaga.update()` should be performed manually before using this list. 


## Optional settings

1. **Specify default groups or languages**

    You can specify a set of default groups and languages in the Manifest, which will be updated on every `Applanga.update()` call. These groups and languages will be added to any that are specified in the call itself, they will *always* be requested.
    The parameter value must be a string with a comma-separated list of groups or languages.

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
2. **Automatic Applanga Settings File Update**

    In case your app's user has no Internet connection, new translation updates can't be fetched, so the Applanga SDK falls back to the last locally cached version. If the app is started for the first time, there will no strings locally cached yet. In this case, the Applanga SDK falls back to the Applanga Settings File, which contains all strings from the moment it was generated, downloaded, and integrated into your app before release.

    To minimize the manual effort of updating the Applanga Settings File, we created a task that triggers the Applanga Settings File generation if changes were made and replaces the old one with the new one in your app.

    **Be aware that the task will work  even if there is no Internet connection. This way, you can continue to develop  offline.** In this case, a warning is printed. To activate the automatic Settings File update, add the following line to your `build.gradle`:

    ```
    // for all builds:
    applanga.settingsFileAutoUpdate = true

    // only for release builds:
    applanga.settingsFileAutoUpdateRelease = true
    ```

    To make sure that the script is running and to see when it does or doesn't update, check the detailed build report in Android Studio after triggering a build.
    There you will find logs for each update step.

    If the file is successfully updated, you should see "Settings File UPDATED" on the log. If it is already up to date, you will see "Settings File UP-TO-DATE" on the log.

3. **Disable automatic string updates when extending the ApplangaApplication Class**

    If you are initializing the SDK by extending or including the provided `ApplangaApplication` class, but you wish to manually control when the SDK communicates with our servers and updates to the latest strings, then you can include the following setting in your app's Manifest: 

    ```xml
    <application>
            ...
            <meta-data android:name="ApplangaInitialUpdate" android:value="false"/>
            ...
    </application>
    ```
	This setting will stop the automatic updates and allow you to call `Applanga.update()` at any time that suits you.
	
4. **Disable Draft Mode**

    If you wish to create a build that can't enable Draft Mode at any time, you can include the following setting in your Manifest.

    ```xml
    <application>
            ...
            <meta-data android:name="ApplangaDraftModeEnabled" android:value="false"/>
            ...
    </application>
    ```
    You can also use the following method at runtime:

    ```
    Applanga.setDraftModelEnabled(bool);
    ```
    
	This will override the setting in the Manifest, but it won't override the [Draft Mode setting from the Applanga dashboard](https://www.applanga.com/docs/applanga-mobile-sdks/draft_on-device-testing#how-to-enable-or-disable-draft-mode-from-the-dashboard).

5. **Convert Placeholder**

    To convert placeholders between iOS and Android styles, you need to enable the following in your Manifest:
	
    ```xml
    <application>
            ...
            <meta-data android:name="ApplangaConvertPlaceholders" android:value="true"/>
            ...
    </application>
    ```

    ***Common placeholder***

    These placeholders won't be converted as they are supported on iOS and Android:

    - Scientific notation `%e` and `%E`
    - `%c` and `%C` Unicode Characters
    - `%f` floating point number
    - `%g` and `%G` computerized scientific notation
    - `%a` and `%A` Floating point numbers
    - Octal integer `%o` (for `%O` see Android to iOS conversion)
    - `%x` and `%X` hexadecimal presentation using lowercase letters (`%x`) or uppercase letters (`%X`)
    - `%d` will remain `%d`
    - Positional placeholders as `%1$s` are converted to `%1$@` and vice-versa


    ***iOS placeholder***
    - All Instances of `%@` will be converted to `%s`
    - Unsupported conversion types by default will be converted to `%s`
    - double `%g` and `%p` are converted to `%d`
    - Objective C integer types like `%i`, `%u`, `%U` are converted to `%d`
    - Floating point numbers `%f` and `%F` will be converted to `%f`
    - iOS `%s` isn't supported and remains `%s` which will be an Android String
    - `%p` iOS Pointer will be converted to Android integer `%d`
    - `%O` Octal integer is converted to `%o`
    - `%D` is converted to `%d`

6. **Language Mapping**

    You can map a locale to another locale. For example, if you don't have `es-CL` added to your dashboard, it usually has a fallback to `es`. But if you want to treat `es-CL` as `es-MX`, then you can add it to the map. Watch out for the log: `ApplangaLanguageMap: es-CL is mapped to es-MX`

    Example:

    ```xml
    <application>
            ...
           <meta-data
            android:name="ApplangaLanguageMap"
            android:value="zh-Hant-HK=zh-HK,es-CL=es-MX" />
            ...
    </application>
    ```

    The locale is mapped in all places in the SDK, except when used in combination with [custom language fallback](#enable-custom-language-fallback). In this case, the custom fallback is performed as set in the referenced XML, no additional mapping occurs for the entries in that XML.
    If you have a locale that maps to another locale that has a custom fallback, this would also work, and the custom fallback will be performed after the mapping.

7. **Enable system language fallback**
	
	By default, Applanga uses a custom device locale order. This meta-data value makes the SDK iterate the languages by using the priority set by the system. This order is used when downloading the translated strings, or if the SDK performs a fallback when a string isn’t found for a specified language. Then the SDK will try to translate strings with the next language in the list. The available possibilities are:

	`applanga`: The default SDK order
	`system`: Use the device system order

	```xml
	<application>
            ...
           <meta-data
            android:name="ApplangaLanguageFallback"
            android:value="system" />
            ...
    </application>
	```

8. **Enable custom language fallback**<a name="enable-custom-language-fallback"></a>
	
	This meta-data value is a resource reference to an XML that allows to set a custom fallback per language. When the SDK needs to translate a key with a given language, it iterates the fallback languages in order. This overrides any other system or default fallbacks only for those languages. Other languages work according to the fallback specified using the `ApplangaLanguageFallback` value (or the default one if it's not set). The fallback is only overridden for the top level language, so it's not possible to "nest" the custom fallbacks.

	Example:

	```xml
	<application>
            ...
           <meta-data
            android:name="ApplangaCustomLanguageFallback"
            android:resource="@xml/custom_fallback" />
            ...
    </application>
	```

    The XML should be structured like this:
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <custom_fallback>
        <language name="es-MX">
            <item>es-MX</item>
            <item>es-US</item>
            <item>es</item>
        </language>
        <language name="de-DE">
            <item>de-AT</item>
            <item>en</item>
        </language>
    </custom_fallback>
	```

9. **Automatic upload of a tag with current app strings**

	If you want to upload a tag with all the local strings in your app, add the following key to the Manifest:
    
    ```xml
    <application>
            ...
           <meta-data
            android:name="ApplangaTagLocalStringsPrefix"
            android:value="some_tag" />
            ...
    </application>
    ```

	This Manifest value would be used as the tag prefix combined with the app version (for example some_tag1.0). Then when you start or run your app with the debugger connected, an automatic tag upload will be performed after `Applanga.update()`. The tag will be created in the project if it doesn't exist yet.

## Jetpack Compose

The Applanga SDK supports `stringResource()` and `stringArrayResource()`.

### Jetpack Compose Screenshots

Jetpack Compose screenshots are less accurate if they have been created manually via the Draft Mode menu. To have accurate string positions you have to pass them to the SDK while doing tests.
A compose test rule is needed as shown in the example below. The example creates two screenshots: one with `showIdMode` enabled (to link all strings correctly to the current tag), and a 'normal' screenshot where all translations are visible.

```kotlin
@get:Rule
val composeTestRule = createAndroidComposeRule(JetpackComposeActivity::class.java)

private fun stringPositionsToString(
    positions: Map<String, Rect>,
    isInShowIdMode: Boolean
): String {
    var json = "{\"ALStringPositions\":[";
    positions.forEach {
        json += "{"
        if (isInShowIdMode) {
            json += "\"key\": \"${it.key}\","
        }
        json += "\"value\": \"${it.key}\","

        json += "\"x\": \"${it.value.left}\"," +
                "\"y\": \"${it.value.top}\"," +
                "\"width\": \"${it.value.width}\"," +
                "\"height\": \"${it.value.height}\"},"
    }

    json = json.substring(0, json.length - 1) + "]}"
    return json
}

private fun getStringPositions(
    nodes: Iterable<SemanticsNode>,
    positions: Map<String, Rect> = mapOf()
): Map<String, Rect> {
    var mutablePositions = positions.toMutableMap();
    nodes.forEach { node ->
        val pos = Rect(node.positionInWindow, node.size.toSize())
        node.config.forEach { configValue ->
            val key = configValue.key.name
            if (key === "Text") {
                val values = configValue.value
                if (values is Collection<*>) {
                    values.forEach { value ->
                        if (value is AnnotatedString) {
                            mutablePositions[value.text] = pos;
                        }
                    }
                }
            }
        }
        mutablePositions =
            getStringPositions(node.children, positions = mutablePositions).toMutableMap()
    }
    return mutablePositions;
}

@Test
fun testUploadStringsWithShowIdMode() {
    Applanga.setShowIdModeEnabled(true)
    composeTestRule.activity.runOnUiThread {
        composeTestRule.activity.recreate()
    };
    val pos = getStringPositions(composeTestRule.onAllNodes(isEnabled()).fetchSemanticsNodes())
    val jsonString = stringPositionsToString(pos, true)
    Applanga.captureScreenshot("jetpack_compose_activity", null, jsonString, null);
    Applanga.setShowIdModeEnabled(false)
}

@Test
fun testUploadStrings() {
    val pos = getStringPositions(composeTestRule.onAllNodes(isEnabled()).fetchSemanticsNodes())
    val jsonString = stringPositionsToString(pos, false)
    Applanga.captureScreenshot("jetpack_compose_activity", null, jsonString, null);
}Internet
```

## Branching

If your project is a branching project, use at least the SDK version 4‎.0.184 and update your Settings File.
The Settings File defines the default branch for your current app.
This branch is used on app start and for update calls.
To be sure branching is working look for the log line: `Branching is enabled.`

To learn more about branching, please check our [Branching](https://www.applanga.com/docs/advanced-features/branching) article.

### Draft Mode

When enabling the Draft Mode, you can switch your branch at runtime - an app restart is required.
You also can use our Draft Mode menu to switch your current branch.
Every screenshot you take is linked to the current branch.

### Production Apps

Already published apps that still use Settings Files without branching and older SDKs will still work and they will use the "main" branch.

 
