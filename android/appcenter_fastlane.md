# Tutorial to implement App Center and Fastlane scripts to Android Project

1. First go to https://appcenter.ms/ and create an account

2. Follow sdk setup steps and implement app center to android project  
([https://docs.microsoft.com/en-us/appcenter/sdk/getting-started/android](https://docs.microsoft.com/en-us/appcenter/sdk/getting-started/android))

    ```Java
    dependencies {
        def appCenterSdkVersion = '3.2.1'
        implementation "com.microsoft.appcenter:appcenter-analytics:${appCenterSdkVersion}"
        implementation "com.microsoft.appcenter:appcenter-crashes:${appCenterSdkVersion}"
    }
    // Add the line below to Application's onCreate() method
    AppCenter.start(getApplication(), "{Your App Secret}",
                      Analytics.class, Crashes.class);
    ```

3. When we launch the app, we should be able see current user on Analytics tab on the AppCenter console.
4. We can manage beta test versions via Distribute tab on the AppCenter console. Advantages and disadvantages are listed below:  

  - Any build_type is acceptable.
  - Invitation to testers could be made on Groups tab.
  - Different package names are acceptable.
  - Unfortunately App Bundle's are not welcome here.
  
## Now its time to implement fastlane

Refer to this [link](https://docs.fastlane.tools/getting-started/android/setup/)

1. Run the commands below to download and setup Fastlane to project

    ```shell
    xcode-select --install

    #Using RubyGems
    sudo gem install fastlane -NV

    #Alternatively using Homebrew
    brew install fastlane

    #Navigate to projects root directory and run
    fastlane init

    #(Optional) Setting up supply - Fetches metadata from play store to upload app
    #supply is a fastlane tool that uploads app metadata, screenshots and binaries to Google Play. You can also select tracks for builds and promote builds to production!
    #Some configurations need to be made on Google Play Console in order to give access to read data to Fastlane
    #After configurations run the code below to check if the configuration is correct
    fastlane run validate_play_store_json_key json_key:/path/to/your/downloaded/file.json
    #Navigate to projects root directory and run
    fastlane supply init

    #Edit ~/.bashrc or ~/.bash_profile in user home directory. (If none of them exist, create one)
    #Add the lines below to the file
    export LC_ALL=en_US.UTF-8
    export LANG=en_US.UTF-8
    ```

2. Fastlane setup is completed now we need to install [app_center plugin](https://github.com/Microsoft/fastlane-plugin-appcenter/)

    > fastlane add_plugin appcenter

3. Obtain api token from [app center](https://appcenter.ms/settings/apitokens) and keep it to use later  
    > Important : api token access type must be  'Full Access' in order to upload version to App center

4. All is ready, now we can run some actions  
    Open `PROJECT_DIR -> fastlane -> Fastfile`  
    > in the file, write actions between  
    > default_platform(:android)  
    > platform :android do  
    > **write actions here**  
    > end  

    - Define flavor based variables

    ```ruby
    before_all do |lane, options|
         if options[:flavor] == "flavorTest"
            ENV["APP_IDENTIFIER"] = "com.example.naci.test"
         elsif options[:flavor] == "flavorProduction"
            ENV["APP_IDENTIFIER"] = "com.example.naci"
         end
    end
    ```

    - Define basic action

    ```ruby
    desc "Generate Production Release Apk"
    lane :generate_my_apk do
        gradle(
           task: "clean assemble",
           flavor: "Production",
           build_type: "Release"
        )
    end
    #Usage on terminal => fastlane generate_my_apk
    ```

    - Define action with options

    ```ruby
    desc "Generate Apk"
    lane :generate_apk do |options|
      UI.message("\n\n\n=====================================\n Generate "+ options[:flavor] +" "+ options[:build_type] +" apk \n=====================================")
        gradle(
           task: "clean assemble",
           flavor: options[:flavor],
           build_type: options[:build_type]
        )
    end
    #usage => fastlane generate_apk flavor:"flavorProduction" build_type:"Release"
    ```

5. (Optional) We can add env file to provide variables for fastlane actions  
    Create .env file (name doesn't matter, you write create any file name) under project root directory  

    Create parameters inside this file like  
    `APPCENTER_API_TOKEN=tokenvalue123`  

    Read file from Fastfile

    ```ruby
    #load variables from .env file in the root if it exists
    if File.exist?('../.env')
         open('../.env', 'r').readlines.each do |l|
           kv = l.split('=')
           ENV[kv[0]] = kv[1].chomp
         end
    end
    ```

6. Some important fastlane actions
    - Define generate_apk action

    ```ruby
    desc "Generate Apk"
    lane :generate_apk do |options|

      gradle(
        task: "clean assemble",
        flavor: options[:flavor],
        build_type: options[:build_type])

      end
    ```

    > gradle action may give errors and didn't run on some cases. You need to download and use JDK 8 inorder to complete<br>Reference [link](https://github.com/google/dagger/issues/1339)

    - Define app center action

    ```ruby
    desc "Fetch Version Number"
    lane :fetch_version do
        UI.message("\n\n\n=====================================\n Fetch latest version: \n=====================================")
        version = appcenter_fetch_version_number(
            api_token: ENV["APPCENTER_API_TOKEN"],
            owner_name: "NaCI",
            app_name: "My-Distribution-Test-App"
        )
        UI.message(version.to_s)
    end
    #usage => fastlane fetch_version
    ```

    - App center distribute action

    ```ruby
    desc "App Center Upload"
    lane :app_center_upload do
        appcenter_upload(
                api_token: ENV["APPCENTER_API_TOKEN"],
                owner_name: "NaCI",
                owner_type: "user",
                app_name: "My-Distribution-Test-App",
                file: "app/flavorProduction/release/app-flavorProduction-release.apk",
                release_notes: sh("cat ../CHANGELOG.md"),
                destinations: "Teta%20Testers",
                destination_type: "group",
                notify_testers: true
        )
    end
    #usage => fastlane app_center_upload
    ```

Thats all. We configured app center and fastlane successfully. Have a nice day :whale2: :fireworks:

Check the [link](https://github.com/microsoft/fastlane-plugin-appcenter/blob/master/fastlane/Fastfile) to see all the examples for app center

## To Show on Code, Refer to

[Repository link](https://github.com/NaCI/DeploymentTestApp)

### [Go back to Android section](../android)
