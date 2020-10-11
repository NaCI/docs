# Tutorial to implement flavor based HMS (Huawei) and GMS (Google) services

Our goal is to create helper methods for hms and gms methods per related flavor and use the services methods on single code base.

## Step 0: Hms docs

Read [hms docs](https://developer.huawei.com/consumer/en/codelab/HMSLocationKit/index.html) and add necessary codes for `build.gradle(project)`

## Step 1: Design Flavor Structure

We need to create new flavors for gms and hms on `build.gradle(app)`

```gradle
flavorDimensions ..., "services"
```

```gradle
productFlavors {
    ...

    gms {
        dimension "services"
        buildConfigField "String", "SERVICE_USED", '"gms"'
        versionNameSuffix "GMS"
    }
    hms {
        dimension "services"
        buildConfigField "String", "SERVICE_USED", '"hms"'
        versionNameSuffix "HMS"
    }
}
```

We can seperate dependencies for each build type

```gradle
dependencies {
    ...

    //Google Services
    gmsImplementation "com.google.android.gms:play-services-location:${googleGmsVersion}"
    gmsImplementation "com.google.android.gms:play-services-maps:${googleGmsVersion}"

    //Huawei Services
    hmsImplementation "com.huawei.agconnect:agconnect-core:${hmsVersion}"
    hmsImplementation "com.huawei.hms:maps:${hmsMapVersion}"
    hmsImplementation "com.huawei.hms:location:${hmsLocationVersion}"
}
```

We only need hms services for hms flavor so at the bottom of build.gradle file appy plugin if hms flavor is active

```gradle
if (getGradle().getStartParameter().getTaskRequests().toString().contains("Hms")) {
    apply plugin: 'com.huawei.agconnect'
}
apply plugin: 'com.google.gms.google-services'
```

## Step 2: Create flavor folders

Create **hms/java** and **gms/java** folders under `src` (You can create your package folders under java folder)

## Step 3: Create Location Helper

We need to use same class with different services dependecies for each flavor (gms and hms) Each class must have same methods for single code base usage

### GMS Location Helper

Create `LocationHelper` under **gms/java**

LocationHelper

```kotlin
import android.annotation.SuppressLint
import android.app.Activity
import android.content.IntentSender.SendIntentException
import android.location.Location
import android.os.Looper
import com.google.android.gms.common.api.ResolvableApiException
import com.google.android.gms.location.*

class LocationHelper @JvmOverloads constructor(
    private val activity: Activity,
    private val globalLocationCallback: GlobalLocationCallback,
    private val requestCode: Int = DEFAULT_REQUEST_CODE
) : LocationCallback() {

    companion object {
        const val DEFAULT_REQUEST_CODE = 9999
    }

    private val fusedLocationClient: FusedLocationProviderClient =
        LocationServices.getFusedLocationProviderClient(activity)
    private val settingsClient: SettingsClient = LocationServices.getSettingsClient(activity)
    private val locationRequest = LocationRequest()

    init {
        locationRequest.interval = 10000
        locationRequest.fastestInterval = 5000
        locationRequest.priority = LocationRequest.PRIORITY_HIGH_ACCURACY
    }

    fun fusedLocationClient() = fusedLocationClient

    @SuppressLint("MissingPermission")
    fun requestLocationUpdatesWithSettingsCheck() {
        val locationSettingsRequest = LocationSettingsRequest.Builder()
            .addLocationRequest(locationRequest)
            .build()

        settingsClient.checkLocationSettings(locationSettingsRequest)
            .addOnSuccessListener {
                fusedLocationClient.requestLocationUpdates(
                    locationRequest,
                    this,
                    Looper.getMainLooper()
                )
                globalLocationCallback.onLocationRequest()
            }
            .addOnFailureListener {
                if (it is ResolvableApiException) {
                    try {
                        it.startResolutionForResult(
                            activity,
                            requestCode
                        )
                    } catch (sendEx: SendIntentException) {
                        // Ignore the error.
                    }
                }
            }
    }

    @SuppressLint("MissingPermission")
    fun requestLocationUpdates() {
        fusedLocationClient.requestLocationUpdates(
            locationRequest,
            this,
            Looper.getMainLooper()
        )
        globalLocationCallback.onLocationRequest()
    }

    override fun onLocationResult(locationResult: LocationResult?) {
        locationResult?.let { locationResultData ->
            for (location in locationResultData.locations) {
                if (location != null) {
                    globalLocationCallback.onLocationResult(location)
                    fusedLocationClient.removeLocationUpdates(this)
                    break
                }
            }
        }
    }

    override fun onLocationAvailability(locationAvailability: LocationAvailability?) {
        locationAvailability?.let {
            val isLocationAvailable = it.isLocationAvailable
            if (!isLocationAvailable) {
                globalLocationCallback.onLocationFailed()
            }
        }
    }

    interface GlobalLocationCallback {
        fun onLocationResult(location: Location)
        fun onLocationFailed()
        fun onLocationRequest()
    }
}
```

### HMS Location Helper

Create `LocationHelper` under **hms/java**

LocationHelper

```kotlin
import android.annotation.SuppressLint
import android.app.Activity
import android.content.IntentSender
import android.location.Location
import android.os.Looper
import com.huawei.hms.common.ResolvableApiException
import com.huawei.hms.location.*

class LocationHelper @JvmOverloads constructor(
    private val activity: Activity,
    private val globalLocationCallback: GlobalLocationCallback,
    private val requestCode: Int = DEFAULT_REQUEST_CODE
) : LocationCallback() {

    companion object {
        const val DEFAULT_REQUEST_CODE = 9999
    }

    private val fusedLocationClient: FusedLocationProviderClient =
        LocationServices.getFusedLocationProviderClient(activity)
    private val settingsClient: SettingsClient = LocationServices.getSettingsClient(activity)
    private val locationRequest = LocationRequest()

    init {
        locationRequest.interval = 10000
        locationRequest.fastestInterval = 5000
        locationRequest.priority = LocationRequest.PRIORITY_HIGH_ACCURACY
    }

    fun fusedLocationClient() = fusedLocationClient

    @SuppressLint("MissingPermission")
    fun requestLocationUpdatesWithSettingsCheck() {
        val locationSettingsRequest = LocationSettingsRequest.Builder()
            .addLocationRequest(locationRequest)
            .build()

        settingsClient.checkLocationSettings(locationSettingsRequest)
            .addOnSuccessListener {
                fusedLocationClient.requestLocationUpdates(
                    locationRequest,
                    this,
                    Looper.getMainLooper()
                )
                globalLocationCallback.onLocationRequest()
            }
            .addOnFailureListener {
                if (it is ResolvableApiException) {
                    try {
                        it.startResolutionForResult(
                            activity,
                            requestCode
                        )
                    } catch (sendEx: IntentSender.SendIntentException) {
                        // Ignore the error.
                    }
                }
            }
    }

    @SuppressLint("MissingPermission")
    fun requestLocationUpdates() {
        fusedLocationClient.requestLocationUpdates(
            locationRequest,
            this,
            Looper.getMainLooper()
        )
        globalLocationCallback.onLocationRequest()
    }

    override fun onLocationResult(locationResult: LocationResult?) {
        locationResult?.let { locationResultData ->
            for (location in locationResultData.locations) {
                if (location != null) {
                    globalLocationCallback.onLocationResult(location)
                    fusedLocationClient.removeLocationUpdates(this)
                    break
                }
            }
        }
    }

    override fun onLocationAvailability(locationAvailability: LocationAvailability?) {
        locationAvailability?.let {
            val isLocationAvailable = it.isLocationAvailable
            if (!isLocationAvailable) {
                globalLocationCallback.onLocationFailed()
            }
        }
    }

    interface GlobalLocationCallback {
        fun onLocationResult(location: Location)
        fun onLocationFailed()
        fun onLocationRequest()
    }
}
```

## Step 4: Use helper on activity

We can use helper class on single activity code which will be uses hms or gms services according to flavor

```kotlin
import android.location.Location

class DashboardActivity : AppCompatActivity(), LocationHelper.GlobalLocationCallback {
    ...

    companion object {
        ...

        const val REQUEST_CHECK_SETTINGS = 999
    }

    private lateinit var locationHelper: LocationHelper

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        ...

        locationHelper = LocationHelper(this, this, REQUEST_CHECK_SETTINGS)
    }

    // Result handled after user enables gps
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        ...

        if (requestCode == REQUEST_CHECK_SETTINGS) {
            if (resultCode == RESULT_OK) {
                requestLocationUpdate()
            }
        }
    }

    // Start location request after permission granted
    @NeedsPermission(Manifest.permission.ACCESS_FINE_LOCATION)
    fun getCurrentLocation() {
        locationHelper.requestLocationUpdatesWithSettingsCheck()
    }

    // GlobalLocationCallback methods
    override fun onLocationResult(location: Location) {
        currentLocation = location
        // Do whatever u want with location information
        progressDialog.setVisibility(View.GONE)
    }

    override fun onLocationFailed() {
        progressDialog.setVisibility(View.GONE)
        // Do whatever u want on error, show snackbar maybe
    }

    override fun onLocationRequest() {
        // Start progressDialog till location data fetch operation ends
        progressDialog.setVisibility(View.VISIBLE)
    }
}
```

And that's all, have a nice coding :technologist: :tada: :confetti_ball:

### REFERENCES

* <https://stackoverflow.com/questions/59974428/have-both-gms-and-hms-in-the-project>
* <https://medium.com/huawei-developers/guide-to-implement-mobile-services-from-different-providers-in-single-codebase-build-variants-b3610fb77fec>
* <https://developer.huawei.com/consumer/en/codelab/HMSLocationKit/index.html>

**< [Go back to Android section](../android)**
