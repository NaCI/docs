# Important Commands for Android Development

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
