# Applanga SDK for Android Localization CHANGELOG
***
*URL:* <https://www.applanga.com> 
***


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