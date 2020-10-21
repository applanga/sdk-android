# Applanga SDK for Android Localization CHANGELOG
***
*Website:* <https://www.applanga.com>

*Applanga Android Documentation:* <https://www.applanga.com/docs-integration/android> 
***

### Version 3.0.139 (21 Oct 2020)
#### Fixed
- Fix for projects using android gradle plugin 4.1.0 with more than 2 build varients

---
### Version 3.0.138 (6 Oct 2020)
#### Added
- Added extra logging to the init proccess to help with debugging database issues

---
### Version 3.0.137 (30 Sep 2020)
#### Fixed
- Support for android gradle plugin 4.1+

---
### Version 3.0.135 (24 Sep 2020)
#### Fixed
- Error when key is too long

#### Added
- Added accessibility ids and changed screenshot overlay flags for appium testing
- Added documentation for manual init

---
### Version 3.0.134 (28 Jul 2020)
#### Fixed
- Gzip https rerquests as default
- added optional interface for providing string positions
- Updated to use the 'maven-publish' plugin for release
- Updated to use latest android sdk and gradle plugin

---
### Version 3.0.133 (11 Jun 2020)
#### Fixed
- Removed accidental debug log on gradle build

---
### Version 3.0.132 (3 Jun 2020)
#### Fixed
- Fixed issues with Android Gradle Plugin 4.x and Android Studio 4.0

---
### Version 3.0.131 (25 May 2020)
#### Fixed
- Automatic removal of language databases that dont exist on the dashboard

---
### Version 3.0.130 (15 May 2020)
#### Fixed
- Plugin now compatible with Java 8

---
### Version 3.0.129 (6 May 2020)
#### Added
- Allow flutter to take screenshots outside of draft mode
- Optional OCR screenshots

---
### Version 3.0.128 (9 Apr 2020)
#### Fixed
- only request supported languages in update

---
### Version 3.0.126 (6 Apr 2020)
#### Fixed
- check for @NoApplanga annotation on methods
- added check for null keys

---
### Version 3.0.125 (2 Apr 2020)
#### Added
- Screenshot interface for Flutter to provide a Bitmap of the current view
- setDraftModelEnabled overide method

---
### Version 3.0.123 (30 Jan 2020)
#### Added
- Sending current language when reporting an issue

---
### Version 3.0.122 (19 Nov 2019)
#### Fixed
- support for localize hint on android.support.design.widget.TextInputLayout & com.google.android.material.textfield.TextInputLayout

#### Added
- Including strings with missing keys when taking a screen shot

---
### Version 3.0.121 (15 Nov 2019)
#### Fixed
- Fix for draft mode crash introduced in 3.0.120

---
### Version 3.0.120 (14 Nov 2019)
#### Fixed
- Fix for getString calls in androidx derived fragments
- Fix for time picker dialog not showing correct default values
- Fix for android app compat and androidx dialogs 

---
### Version 3.0.117 (2 Oct 2019)
- added check for keys longer than 997 bytes to meet database requirements
- added check for new lines in keys
- added ApplangaDraftModeEnabled setting

---
### Version 3.0.115 (11 Sep 2019)
#### Added
- added ApplangaInitialUpdate setting
- updated viewpump to allow for android target 29

---
### Version 3.0.114 (18 Jul 2019)
#### Fixed 
- added support for sr and sr-Latn

---
### Version 3.0.113 (7 Jun 2019)
#### Fixed 
- fixed deprection warning regarding getGenerateBuildConfigProvider
- fixed deprection warning regarding getMergeResourcesProvider
- fixed support for Robolectric tests (see documention)
- fixed device id for non playstore builds
- reduced Proguard obfuscation

---
### Version 3.0.112 (2 May 2019)
#### Fixed 
- missing string upload if app is using some activities with react native or flutter

---
### Version 3.0.110 (6 Feb 2019)
#### Fixed 
- missing ids on some screenshots with Hints or EditTextView

---
### Version 3.0.109 (31 Jan 2019)
#### Fixed 
- crash on menus with not string references

---
### Version 3.0.108 (30 Jan 2019)
#### Fixed 
- viewpump initialization if it was initialized earlier

---
### Version 3.0.107 (4 Jan 2019)
#### Fixed 
- plugin preference parser

---
### Version 3.0.106 (4 Jan 2019)
#### Fixed 
- more explicit call of plugin methods

---
### Version 3.0.105 (2 Jan 2019)
#### Fixed 
- gradle build improvements

---
### Version 3.0.104 (18 Dec 2018)
#### Fixed
- preference localisation issues

#### Added
- preference example to documentation

---
### Version 3.0.103 (17 Dec 2018)
#### Fixed
- Applanga logs adjusted

---
### Version 3.0.102 (14 Dec 2018)
#### Added
- using ViewPump to intercept view inflation
- removed layout manipulation
- improved screenshot string detection
- build and runtime performance improvements

---
### Version 2.0.100 (9 Nov 2018)
#### Added
- reinitialization on database corruption at runtime

---
### Version 2.0.99 (25 Oct 2018)
#### Fixed
- getStringArray with String reference in String

---
### Version 2.0.98 (23 Oct 2018)
#### Added
- Support for String reference in String
- Support for CDATA-String notation on the dashboard
- Android split binary support

#### Fixed
- Draft changes were not actualizing while in draft mode
- Missing applanga settings file overlay fixed (also for react-native)
- localizeMap() removes the string entry from its response if no translation is available instead of returning its key as value - for custom fallback handling (react-native)

---
### Version 2.0.96 (10 Oct 2018)
#### Fixed
- Gradle 4.6 issues with some layout files

---
### Version 2.0.95 (9 Oct 2018)
#### Added
- Gradle 4.6 functionality
- showDraftModeDialog(Activity) to toggle the draft mode dialog without gesture

---
### Version 2.0.94 (25 Sep 2018)
#### Fixed
- Handling of corrupt sqlite databases

---
### Version 2.0.93 (14 Aug 2018)
#### Added
- React-Native support

---
### Version 2.0.92 (6 Aug 2018)
#### Fixed
- Android P database crash

---
### Version 2.0.91 (25 Jun 2018)
#### Changed
- Updated WebView Documentation
- Widget Support Documentation

---
### Version 2.0.89 (18 Jun 2018)
#### Fixed
- fixed internal getLocal-Call

---
### Version 2.0.87 (31 May 2018)
#### Added
- JavaDoc for Applanga.java
- BackgroundThread disabled by default
- Reflection disabled for better Android O performance
- Delta Updates

#### Fixed
- Refactoring
- PopupMenu issues
- Issue missing string upload for string-arrays

---
### Version 2.0.86 (8 May 2018)
#### Added
- Usage of Androids transform API

#### Fixed
- Applanga plugin error with Android plugin 3.1.2 fixed

---
### Version 2.0.83 (2 May 2018)
#### Fixed
- TextInputLayout hint issue
- overwrite local string cache if settings file version is newer even without calling Applanga.update()

---
### Version 2.0.82 (18 Apr 2018)
#### Added
- added sources.jar with Applanga.java

#### Fixed
- translation issues while using kotlin extension functions in conjunction with adapters
- removed unused applanga handler thread if RunBackgroundThread is turned off

---
### Version 2.0.81 (4 Apr 2018)
#### Fixed
- Android O performance improvements
- Realm incompatibilities in bytecodemanipulation
- gradle parallel build issues for string and layout interdependencies between different modules

---
### Version 2.0.80 (28 Mar 2018)
#### Fixed
- possible crash on json parsing on pixel
- android oreo screenshot permission request
- android studio 3.1 support

---
### Version 2.0.78 (13 Mar 2018)
#### Fixed
- issues that where introduced with gradle parallel build support

---
### Version 2.0.77 (8 Mar 2018)
#### Added
- new option to skip layout & bytecode manipulation

#### Fixed
- untranslated menus inflated from DataBindingUtil.inflate & MenuInflater.inflate
- classpath crash if you used kotlin class from within java

---
### Version Version 2.0.76 (27 Feb 2018)
#### Fixed
- resolved issue where custom EditText setText and setHint methods crashed

---
### Version 2.0.75 (20 Feb 2018)
#### Fixed
- gradle parallel build support

---
### Version 2.0.74 (2 Feb 2018)
#### Changed
- added proxy support
- databinding support

#### Fixed 
- hint localization
- support for Java derived classes in kotlin

---
### Version 2.0.73 (24 Jan 2018)
#### Changed
- added automatic localization support for Android AutoCompleteTextView

---
### Version 2.0.72 (4 Jan 2018)
#### Changed
- added kotlin support for automated getString calls etc. with the applanga plugin

---
### Version 2.0.71 (12 Dec 2017)
#### Changed
- autotranslate View.setText and setHint calls
- ignore strings from donottranslate.xml files
- new flag to enable automatic settings file update

---
### Version 2.0.70 (23 Nov 2017)
#### Changed
- fixed MenuInflater crash on localizing menu without title

---
### Version 2.0.69 (20 Nov 2017)
#### Changed
- fixed endless loop on setContentView override

---
### Version 2.0.68 (15 Nov 2017)
#### Changed
- added changelog

---
### Version 2.0.67 (3 Nov 2017)
#### Changed
- plugin build time improvements

---
### Version 2.0.66 (26 Oct 2017)
#### Changed
- Android Studio 3 Release support

---
### Version 2.0.65 (19 Oct 2017)
#### Fixed
- localize Fragment issue

---
### Version 2.0.64 (18 Oct 2017)
#### Changed
- call localizeView onCreate

---
### Version 2.0.63 (9 Oct 2017)
#### Fixed
- menu translation issue

---
### Version 2.0.62 (4 Oct 2017)
#### Added
- roboelectric support

---
### Version 2.0.61 (3 Oct 2017)
#### Fixed
- minor draft mode memory leak

---
### Version 2.0.60 (2 Oct 2017)
#### Fixed
- return empty string issue

---
### Version 2.0.59 (26 Sep 2017)
#### Fixed
- quantity string fallback

---
### Version 2.0.57 (25 Sep 2017)
#### Fixed
- lint issue
- missing settingsfile overlay

---
### Version 2.0.53 (4 Sep 2017)
#### Changed
- basic binary style support

---
### Version 2.0.51 (10 Aug 2017)
#### Changed
- Android Studio 3 Beta Support

---
### Version 2.0.50 (2 Aug 2017)
#### Changed
- removed extensive logging

---
### Version 2.0.49 (1 Aug 2017)
#### Changed
- switched from gradle script to plugin
- deprecated - getRessources
- updated README for new plugin integration
- lint checks for sdk and plugin version

---
### Version 1.0.48 (10 Jul 2017)
#### Fixed
- settingsfile missing dialog
- high memory usage

---
### Version 1.0.43 (19 May 2017)
#### Fixed
- recyclerview performance

---
### Version 1.0.42 (15 May 2017)
#### Changed
- support for recyclerviews

---
### Version 1.0.41 (10 May 2017)
#### Fixed
- rotation issue
- only print setting values if they are set
- performance issue

---
### Version 1.0.40 (24 Apr 2017)
#### Changed
- low memory handling
- show local strings if database is not initialized
- print logmessage if neither debugger nor draft mode is connected/enabled
- make sure only app strings are uploaded to the dashboard

---
### Version 1.0.39 (10 Mar 2017)
#### Changed
- added android nougat support
- support for different resource classes per activity

---
### Version 1.0.36 (26 Jan 2017)
#### Changed
- allow swipe in both directions for screenshot menu

---
### Version 1.0.35 (8 Dec 2016)
#### Changed
- option to read packagename in manifest

---
### Version 1.0.34 (24 Nov 2016)
#### Fixed
- connectivity issue

---
### Version 1.0.32 (23 Nov 2016)
#### Fixed
- connectivity issue

---
### Version 1.0.31 (22 Nov 2016)
#### Fixed
- docs indentation and counts
- draft mode setting deactivation
- screenshot issues

---
### Version 1.0.29 (20 Oct 2016)
#### Changed
- smart screenshot capture for subviews

---
### Version 1.0.27 (8 Sep 2016)
#### Changed
- improved settingfile loading

---
### Version 1.0.25 (25 Aug 2016)
#### Fixed
- minor fixes

---
### Version 1.0.24 (12 Aug 2016)
#### Changed
- added pro guard setting to prevent warnings caused by "-keepattributes InnerClasses"

---
### Version 1.0.23 (9 Aug 2016)
#### Fixed
- log typo
- getString for ressource ids

---
### Version 1.0.22 (8 Aug 2016)
#### Added
- internal mapping for iw->he , ji->yi, in->id
- getString for ressource ids

#### Fixed
- pluralization issues
- test support
- proguard pluralrules fix
- database cursor not closed

---
### Version 1.0.21 (2 Jun 2016)
#### Changed
- setLanguage improvements

#### Fixed
- API level 10 issues

---
### Version 1.0.19 (30 May 2016)
#### Changed
- preferences localization support
- localizeView threading support

---
### Version 1.0.18 (13 May 2016)
#### Changed
- moved init into seperate thread

#### Fixed
- strict mode failure

---
### Version 1.0.16 (4 May 2016)
#### Fixed
- string arrays cut off on empty values

---
### Version 1.0.15 (3 May 2016)
#### Fixed
- crash on new API level

---
### Version 1.0.14 (26 Apr 2016)
#### Fixed
- marshmallow support
- localizeView to cover views that are extend view groups and have setText/setHint methods

---
### Version 1.0.13 (14 Mar 2016)
#### Changed
- set min sdk to 9
- performance optimizations

#### Fixed
- wrong log output

---
### Version 1.0.12 (16 Feb 2016)
#### Changed
- only collect missing strings if debugger is connected or app is in draft mode
- aligned log output on iOS and android
- check if app is beeing debugged

---
### Version 1.0.11 (14 Jan 2016)
#### Fixed
- group update issue
- default language fallback issue

---
### Version 1.0.10 (16 Dec 2015)
#### Changed
- setLanguage method to switch language
- string-array support
- print used settings

#### Fixed
- threading issue
- documentation
- API 23 issue
- air sdk fix
 
---
### Version 1.0.9 (3 Nov 2015)
#### Added
- basic support for runtime language switching

---
### Version 1.0.6 (14 Oct 2015)
#### Fixed
- build script

---
### Version 1.0.3 (10 Oct 2015)
#### Changed
- webview performance improvements
- show dialog when no settingsfile could be found

#### Fixed
- empty value issue
- draft mode exception

---
### Version 1.0.2 (24 Sep 2015)
#### Fixed
- group updates
- log message spam

---
### Version 1.0.1 (15 Sep 2015)
#### Added
- WebView support
- air sdk support
- support for non translateable strings
- pluralization support

#### Fixed
- Initialization order
- missing string upload

---
### Version 1.0.0  (13 Mar 2015)
#### Changed
- Initial Release

--- 
