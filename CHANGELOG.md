# Applanga SDK for Android Localization CHANGELOG
***
*Website:* <https://www.applanga.com>

*Applanga Android Documentation:* <https://www.applanga.com/docs-integration/android> 
***

### Version 4.0.207 (13 Nov 2024)
#### Added
- internal exception logging
- applanga sdk + plugin version warnings

---
### Version 4.0.202 (8 May 2024)
#### Fixed
- setHint NullPointerException

---
### Version 4.0.201 (18 Apr 2024)
#### Added
- public method to retrieve available languages on the dashboard

---
### Version 4.0.200 (12 Mar 2024)
#### Added
- improved Appium Screenshot support
- gradle configuration cache support

---
#### Fixed
- removed unnecessary warnings for Flutter and React-Native builds
- missing string upload on windows machines

---
### Version 4.0.199 (25 Jan 2024)
#### Added
- shrinked very first Applanga.update() response
- smaller general code optimizations

#### Fixed
- fixed possible caching issues

---
### Version 4.0.197 (7 Dec 2023)
#### Fixed
- workaround for specific androidx version bug: IllegalArgumentException in LocaleList
- setLanguage persistency issue

---
### Version 4.0.196 (10 Nov 2023)
#### Fixed
- NullPointerException for specific AndroidManifest configurations 
- reduced AAR size by removing accidentally added test resources 

---
### Version 4.0.195 (12 Oct 2023)
#### Added
- option for system language fallback
- more performant settings file handling
- option for custom fallbacks
- option for local string tagging in draft mode

#### Fixed
- new language on the dashboard sync on first app start

---
### Version 4.0.194 (20 Jul 2023)
#### Added
- branching support for Flutter and React-Native 

--- 
### Version 4.0.193 (29 Jun 2023)
#### Fixed
- fixed name collisions for applanga_meta if modules (or sub/sub-sub-modules) have the same name
#### Added 
- improved applanga_meta clean task

--- 
### Version 4.0.191 (31 May 2023)
#### Fixed
- duplicated menu with same item id in sub menu was wrongly displayed in some circumstances

---
### Version 4.0.190 (25 May 2023)
#### Fixed
- wrongly showing MissingApplangaSDKException

#### Added
- improved settings file handling

---
### Version 4.0.189 (11 May 2023)
#### Fixed
- proguard duplicate class build error

---
### Version 4.0.188 (26 Apr 2023)
#### Fixed
- specific NavigationMenu.inflateMenu crash at runtime

--- 
### Version 4.0.187 (25 Apr 2023)
#### Fixed
- specific NavigationMenu.inflateMenu crash at build time 

--- 
### Version 4.0.186 (21 Apr 2023)
#### Fixed
- cleaned up some build warnings
- handles translation for custom NavigationMenu.inflateMenu implementations
- fixed translations for menus with sub-items
- fixed dialog title translation for list preferences

#### Added
- added support for referenced (untranslatable) strings as preferences key

---
### Version 4.0.184 (2 Mar 2023)
#### Added
- settingsFileAutoUpdateRelease to update the settings file only for release builds
- support for per-app language preferences
- new Draft Mode dialog

---
### Version 4.0.183 (22 Dec 2022)
#### Fixed
- failure of screenshot uploads

---
### Version 4.0.182 (6 Dec 2022)
#### Added
- getTranslation, getTranslationArray, getQuantityTranslation as a replacement of the deprecated Applanga.getString methods

---
### Version 4.0.181 (24 Nov 2022)
#### Fixed
- Resources$NotFoundException for dynamic numeric string ids

---
### Version 4.0.180 (10 Nov 2022)
#### Fixed
- unit test detection failed for different setups

---
### Version 4.0.179 (3 Nov 2022)
#### Added
 - NavController label support
#### Fixed
 - Applanga.setLanguage() for unit tests returns false

---
### Version 4.0.178 (27 Oct 2022)
#### Fixed
- Some configuration broke RTL layouts and TextView TextAlignment center

---
### Version 4.0.177 (26 Oct 2022)
#### Fixed
- Robolectric support and unit tests

---
### Version 4.0.175 (11 Oct 2022)
#### Added
- basic Robolectric support with limitations due to the AGP Plugin

---
### Version 4.0.174 (6 Oct 2022)
#### Fixed
- resolved proguard issues with fragment.getString in specific lambda methods

---
### Version 4.0.173 (5 Oct 2022)
#### Fixed
- fragment.getString fixed in specific typed lambda methods

---
### Version 4.0.172 (27 Sep 2022)
#### Fixed
- overriding attachBaseContext stopped views to be translated in some cases

---
### Version 4.0.171 (15 Sep 2022) 
#### Fixed
- Java 11 issues with ASM transformer

---
### Version 4.0.170 (14 Sep 2022) 
#### Added
- experimental support for kotlin multi-platform modules

#### Fixed
- sync issues if plugin version is declared in top-level build.gradle

---
### Version 4.0.169 (9 Sep 2022) 
#### Added
- better AGP version 7+ support
- use of the new transforms API which make the build faster and more stable
- automatic SDK initialization on app start
- support for plugins{} notation
- removed internal Applanga APIs from the Applanga Interface

#### Fixed
- broken jar / zip error
- unstable builds because internal AGP APIs in complex build scenarios

---
### Version 3.0.167 (21 Jul 2022) 
#### Fixed
- build errors when using custom views with overloading setText method

---
### Version 3.0.166 (12 Jul 2022) 
#### Fixed
- RTL layouts were ignored for specific cases

---
### Version 3.0.165 (22 Jun 2022) 
#### Fixed
- improved internal applanga_meta xml parsing time -> affecting Applanga.init() startup time

---
### Version 3.0.164 (8 Jun 2022) 
#### Fixed
- improved Applanga.init() startup time
- fixed wrong missing string upload edge case while using instant run

### Version 3.0.163 (19 Apr 2022) 
#### Fixed
- screenshot menu keyboard for tag name not showing 

---
### Version 3.0.162 (14 Apr 2022) 
#### Fixed
- missing string upload for android gradle plugin 7.1.3 if package name differs from application id
- menu and preferences translation if using android gradle plugin 7.1.3
- activity.setTitle(resourceId)

---
### Version 3.0.161 (9 Mar 2022) 
#### Added
- localizeMap support for empty (null) translations

---
### Version 3.0.160 (24 Feb 2022) 
#### Fixed
- removed unused reference to bluetooth to fix API 31 warnings

---
### Version 3.0.159 (17 Feb 2022) 
#### ADDED
- language mapping option
- zh-Hant-HK support

---
### Version 3.0.158 (4 Feb 2022) 
#### FIXED
- case where id was shown instead of translation at startup

---
### Version 3.0.156 (20 Jan 2022) 
#### ADDED
- improved placeholder conversion documentation

#### FIXED
- conversion `%g` remains `%g`
- conversion `%U` converts to `%d`
- conversion `%D` converts to `%d`

---
### Version 3.0.155 (18 Jan 2022) 
#### FIXED
- placeholder conversion database creation for multiple languages bug

---
### Version 3.0.154 (6 Jan 2022) 
#### FIXED
- placeholder conversion bug fixed 

---
### Version 3.0.153 (17 Dec 2021) 
#### ADDED
- added support for JetpackCompose
- improved Actionbar support
- added exit button for screenshot menu
- improved MaterialAlertDialogBuilder Applanga plugin support
- Applanga setLanguageAndUpdate()
- set minSdkVersion from 9 to 11

---
### Version 3.0.151 (28 Oct 2021) 
#### FIXED
- support for simulator M1 macbook

---
### Version 3.0.150 (2 Sep 2021) 
#### ADDED
- Applanga Show Id Mode
- Applanga Convert Placeholder

---
### Version 3.0.149 (25 Aug 2021) 
#### ADDED
- setDraftModelEnabled() is deprecated and renamed to setDraftModeEnabled()

#### FIXED
- @NoApplanga was not respected on class level

---
### Version 3.0.147 (24 Jun 2021) 
#### ADDED
- first gradle 7+ and android plugin 7+ support

---
### Version 3.0.146 (16 Jun 2021)
#### FIXED
- improved fix for random gradle build issue 

---
### Version 3.0.145 (11 Jun 2021)
#### Fixed
- resolved random gradle build issue

---
### Version 3.0.144 (14 May 2021) 
#### Fixed
- Auto detection of UITexts using androidX on device
- crash in localizedStringsForCurrentLanguage

---
### Version 3.0.143 (24 Mar 2021) 
#### Added
- Extra logging for initial database errors

---
### Version 3.0.142 (18 Mar 2021)
#### Fixed
- Automatic detection of androidx uitests for automated screenshots
- Changed package name of flutter plugin to work with flutter V2
- Support for menus with item groups

#### Added
- Support for AndroidX navigation view menu

---
### Version 3.0.141 (6 Jan 2021)
#### Added
- Draft menu redesign
- added method 'localizedStringsForCurrentLanguage'
- Support for draft mode, plugin and ocr screenshots in jetpack compose  

---
### Version 3.0.140 (26 Nov 2020)
#### Fixed
- Moved ALLOG init so that it happens before anything else, thus enabling loggign at all stages of init process.

---
### Version 3.0.139 (21 Oct 2020)
#### Fixed
- Moved ALLOG init so that it happens before anything else, thus enabling loggign at all stages of init process.

---
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
