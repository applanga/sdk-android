#Applanga SDK for Android
***
*Version:* 1.0.1

*URL:* <http://applanga.com> 
***

##Installation
1. Download the latest release of the Applanga Android SDK from [Github](https://github.com/applanga/sdk-android/blob/mvn-repo/maven/releases/com/applanga/android/Applanga/1.0.1/Applanga-1.0.1.aar?raw=true). Unzip it, then drag and drop Applanga.aar into your project's app/libs folder. **Note**: if you are using drag and drop on OSX you may need to hold down the option key.

2. Add the Applanga SDK to your list of dependencies in your **build.gradle** script.

        compile(name:'Applanga', ext:'aar')

3. Make sure that the **build.gradle** script contains a configuration that marks the libs directory as a flatDir:

        repositories {
          flatDir {
            dirs 'libs'
          }
        }

4. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your App internet access, which is needed for Applanga to function. 

        <uses-permission android:name="android.permission.INTERNET" />


##Configuration
1. Download the Applanga *settingsfile* for your app from the Applanga App Overview by clicking ***[Download Settings]***. 
 
2. Add the *settingsfile* to your apps resources res/raw directory.


##Usage
Once Applanga is integrated and configured it is synchronizing your local strings with the Applanga dashboard every time you start your app or if new missing strings are found.

1. The easiest way to initialize ```Applanga``` in your Application is by extending your Application from ```ApplangaApplication```.

		import com.applanga.android.ApplangaApplication;

        public class MyApplication extends ApplangaApplication {
			...
		}

2. To get notified on Localization Updates (e.g. to show a LoadingScreen at beginning of your App) override the ```onLocalizeFinished``` method.

		public class MyApplication extends ApplangaApplication {
			...
        	@Override
    		public void onLocalizeFinished(boolean success) {
				//do something on finished loacalization
    		}
    		...
    	}

3. To have a ```Menu``` automatically localized use the Applanga ```MenuInflater```.
		
		@Override
    	public boolean onCreateOptionsMenu(Menu menu) {
        	// Inflate the menu; this adds items to the action bar if it is present.
        	Applanga.getMenuInflater().inflate(R.menu.my, menu);
        	return true;
    	}

4. You should also override ```getMenuInflater``` method of your Activitys to have menus automatically translated if possible.

		public class MyActivity extends ActionBarActivity {
			...
			@Override
   			public android.view.MenuInflater getMenuInflater() {
        		return com.applanga.android.Applanga.getMenuInflater();
    		}
    		...
    	}

5. **Code Localisation**
 
	5.1 **Strings** 

		// get translated string for the current device locale
        Applanga.getString("APPLANGA_ID");

	5.2 **Arguments**
        
        // get translated string with formatted arguments
        // using the default string format %s %d etc
        // @see http://developer.android.com/reference/java/util/Formatter.html
        Applanga.getString("APPLANGA_ID", "arg1", "arg2", "etc.");
        
     **Named Arguments**
                
        // if you pass a string map you can get translated string
        // with named arguments. %{someArg} %{anotherArg} etc.
        Map<String, String> args = new Map<String, String>();
        args.put("someArg","awesome");
        args.put("anotherArg","crazy");
        Applanga.getString("APPLANGA_ID", args);
        
    Example:
    
    *APPLANGA_ID* = *"This value of the argument called someArg is %{someArg} and the value of anotherArg is **%{anotherArg}**. You can reuse arguments multiple times in your text wich is **%{someArg}**, **%{anotherArg}** and **%{someArg}.**"*	
    
    gets converted to:
    
    *"This value of the argument called someArg is awesome and the value of anotherArg is crazy. You can reuse arguments multiple times in your text wich is awesome, crazy and awesome."*    
        
	5.3 **Pluralisation**
		
		// get translated string in given pluralisation rule (one)
		Applanga.getString("APPLANGA_ID", Applanga.PluralRule.ALPluralRuleOne)
		
	Available pluralisation rules:
	
        Zero,
        One,
        Two,
        Few,
        Many,
        Other
		
	you can also specify a quantity and Applanga will pick the best pluralisation rule based on: [http://unicode.org/.../language_plural_rules.html	](http://unicode.org/repos/cldr-tmp/trunk/diff/supplemental/language_plural_rules.html)
			
		// get a string in the given quantity
		Applanga.getQuantityString("APPLANGA_ID", quantity);
		
	In the dashboard you create a **puralized ID** by appending the Pluralisation rule to your **ID** in the following format: ```[zero]```, ```[one]```,```[two]```,```[few]```,```[many]```, ```[other]```.
	
	So the ***zero*** pluralized ID for ***"APPLANGA_ID"*** is ***"APPLANGA_ID[zero]"***
               

6. **UI Localisation**
	
	After you've made programmatic changes to your UI Elements you should call one of the following methods to update you UI.
	
	6.1 **Activities**
	
	Activity localisation is always automatically triggered when a Activity is loaded or resumed. (onResume)
	
	If you want to trigger it manually use:
	
		// localize a Activity and all its Views
		Applanga.localizeActivity(activity);
		
	6.2 **Views**
	
		// localize a View and all its children
		Applanga.localizeView(view);
		
	6.3 **Menus**
		
		// localize a Menu
		Applanga.localizeMenu(menu);

7. **Update Content**
 
	To trigger a full update call:
	 	
	 	Applanga.update(new ApplangaCallback() {
            @Override
            public void onLocalizeFinished(boolean success) {
            	//called if update is complete    
            }
        });
    	
    To trigger a update for a subset of groups and languages call:
	 	
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
    			
8. **WebViews**
	
	Applanga can also translate content in your WebViews if they have JavaScript enabled.
	
		WebView myWebView = (WebView) findViewById(R.id.webview);
        myWebView.getSettings().setJavaScriptEnabled(true);

        myWebView.loadUrl("html_file_path"); 
        
        
	To initalize Applanga for your webcontent you need to initialize Applanga from JavaScript:
	
		<script type="text/javascript">
    		var applanga = Applanga({});
		</script>
		
	7.1 **Strings**
		
	The inner text and html of tags wich have a ```applanga-text="APPLANGA_ID"``` attribute will be replaced with the translated value of ***APPLANGA_ID***
	
		<div applanga-text="APPLANGA_ID">
			***This will be replaced with the value of APPLANGA_ID***
		</div>
	
	
	7.2 **Arguments**
	
	You can pass arguments with the ```applanga-args``` attribute.
	By default the arguments are parsed as a comma seperated list wich then will replace fields as %{arrayIndex}.  
	
		<div applanga-text="APPLANGA_ID" applanga-args="arg1,arg2,etc">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with arguments***
		</div>
	
	To define a different separator instead of ```,``` e.g. if your arguments contain commas use ```applanga-args-separator```.
	
		<div applanga-text="APPLANGA_ID" applanga-args="arg1;arg2;etc"
		 applanga-args-separator=";">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with arguments***
		</div> 
		
	1 Dimensional **JSON** Objects can also be used as ***Named Arguments*** if you add ```applanga-args-separator="json"```
 	
		<div applanga-text="APPLANGA_ID" 
		 applanga-args="{'arg1':'value1', 'arg2':'value2', 'arg3':'etc'}"
		 applanga-args-separator="json">
			***This will be replaced with the value of APPLANGA_ID***
			***and formatted with json arguments***
		</div> 
	
	
	7.3 **Pluralisation**
		
	To pluralize a html tag you can pass the ```applanga-plural-rule``` attribute with the value ```zero```, ```one```, ```two```, ```few```, ```many``` and ```other```.
	
		<div applanga-text="APPLANGA_ID" applanga-plural-rule="one">
			***This will be replaced with the pluralized value of APPLANGA_ID***
		</div> 
		
	You can also pluralize by quantity via ```applanga-plural-quantity```	
		
		<div applanga-text="APPLANGA_ID" applanga-plural-quantity=42>
			***This will be replaced with the pluralized value of APPLANGA_ID***
		</div> 	
	
	7.4 **Update Content**
	
	To trigger a content update from a WebView use javascript:
		
		applanga.updateGroups("GroupA, GroupB", "de, en, fr", function(success){
        	//called if update is complete
    	});				
8. To enable support for **Draft Mode** in your application, override the ```dispatchTouchEvent``` method in your targeted activity and forward the event to Applanga.dispatchTouchEvent. To enable Draft Mode, hold down four fingers for four seconds in this activity. A dialog appears asking you to enter a key code, which is the first four characters of your app secret and can also be found in your app's main view on the dashboard. When the right key is entered, the application will switch to Draft mode and quit, restart it manually. The Draft mode can be disabled in the same way as it was enabled. Please be aware that not all Android devices support multitouch with four fingers.

		@Override
    	public boolean dispatchTouchEvent(MotionEvent ev) {
        	Applanga.dispatchTouchEvent(ev, this);
			return super.dispatchTouchEvent(ev);
    	}
