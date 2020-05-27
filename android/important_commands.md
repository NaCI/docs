### SHA1 of debug keystore

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android 
```

### Add adb to path
1. Locate sdk path from SDK Manager on Android Studio
2. Open ~/.bash_profile file (create if not exist) (on Catalina OS it is ~/.zprofile)
3. export PATH=<Path to sdk>/platform-tools/:$PATH  
4. Save and exit
5. Run the `source ~/.bash_profile` command to refresh the terminal (or ~/.zprofile)
  
### Kill app on background -like OS does to create some space on RAM-
` adb shell ps -A |grep mypackage` -> get the package name of my app from running proccesses in emulator
`adb shell am kill {app_package_name}` -> app package name is like : com.example.android.dessertpusher 

### Git reflog
Git reflog<br>
Git reflog show {branch_name}<br>
Git reset —hard HEAD@{7}<br>

### ANDROID STUDIO KISAYOLLAR
Cmd + alt + O => sınıf, metod ya da field buluyor<br>
Cmd + j => kısayol yazdırma (foreach gibi)<br>
Cm + shift + a => studio aksiyonları arama<br>
Cmd + n => getter setter<br>
Control + o => overridable method list<br>
Cmd + alt + t => surround with<br>
