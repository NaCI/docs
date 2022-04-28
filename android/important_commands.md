# Important Commands for Android Development

## General

- Son versiyonlu kütüphaneleri arama: [Link](https://androidreposearch.netlify.com)
- Play services libraries versions: [Link](https://developers.google.com/android/guides/releases)
- Test server side push notifications: [Link](https://github.com/onmyway133/PushNotifications)
- Debug Gradle File: [Link](https://docs.gradle.org/current/userguide/img/remote-debug-gradle.gif)
- Usage of material: [Link](https://medium.com/mindorks/upgrading-to-material-components-ebc21ac4e95a)
- Room annotations: [Link](https://developer.android.com/reference/androidx/room/package-summary#annotations)
- SimpleDateFormat: [Link](https://developer.android.com/reference/java/text/SimpleDateFormat)
- Color Hex to Name: [Link](https://www.color-blindness.com/color-name-hue/)
- Compress image files: [Link](https://tinypng.com)
- Android studio theme: [Plugin](https://plugins.jetbrains.com/plugin/8006-material-theme-ui)
- Android device screen share with computer: [Link](www.vysor.io)
- Online markdown editor: [Link](https://dillinger.io)
- Online Json formatter: [Link](https://jsonformatter.curiousconcept.com)
- All design pattern samples: [Repo](https://github.com/iluwatar/java-design-patterns)
- Run android libraries without using android studio: [Dryrun](https://github.com/cesarferreira/dryrun)
- Get free images : [Unsplash](unsplash.com)
- Observe app network requests : [Httptoolkit](https://httptoolkit.tech)
- All languages known from Github : [Link](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)
- Android beginner guide : [Link](https://developers.google.com/android/for-all/vocab-words)
- Android github library share : [Jitpack](https://jitpack.io)
- Android gitignore sample : [Github link](https://github.com/github/gitignore)
- Mock api responses : [mocky.io](https://designer.mocky.io)
- Resource naming standarts : [resourcenaming](https://jeroenmols.com/blog/2016/03/07/resourcenaming/)
- Create or show badges : [shields.io](https://shields.io)
- AndroidX libraries latest releases : [Link](https://androidx.tech)
- Online UUID Generator : [Link](https://www.uuidgenerator.net/version4)
- Test mobile deeplink : [Link](https://halgatewood.com/deeplink/)
- Proguard playground : [Link](https://playground.proguard.com)
- Mobile Certificate Pinning Generator : [Link](https://approov.io/tools/static-pinning/)
- Mock Service Requests : [Retromock](https://infinum.com/blog/retromock-retrofit-calls/)

## SHA1 of debug keystore

``keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android``

[Stackoverflow link](https://stackoverflow.com/questions/15727912/sha-1-fingerprint-of-keystore-certificate){:target="_blank"}

## Automize app signing

[https://developer.android.com/studio/publish/app-signing#secure-shared-keystore](https://developer.android.com/studio/publish/app-signing#secure-shared-keystore){:target="_blank"}

## Add adb to path

1. Locate sdk path from SDK Manager on Android Studio
2. Open ~/.bash_profile file (create if not exist) (on Catalina OS it is ~/.zprofile)
3. export `PATH=<Path to sdk>/platform-tools/:$PATH`
4. Save and exit
5. Run the `source ~/.bash_profile` command to refresh the terminal (or ~/.zprofile)
  
## Kill app on background -like OS does to create some space on RAM-

`adb shell ps -A |grep mypackage` -> get the package name of my app from running proccesses in emulator
`adb shell am kill {app_package_name}` -> app package name is like : com.example.android.dessertpusher

## Git reflog

> Git reflog  
> Git reflog show {branch_name}  
> Git reset —hard HEAD@{7}

## ANDROID STUDIO KISAYOLLAR

- Cmd + alt + O => sınıf, metod ya da field buluyor
- Cmd + j => kısayol yazdırma (foreach gibi)
- Cm + shift + a => studio aksiyonları arama
- Cmd + n => getter setter
- Control + o => overridable method list
- Cmd + alt + t => surround with

**< [Go back to Android section](../android)**
