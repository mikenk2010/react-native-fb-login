# How to integrate FB SKD to React Native

### Initial project and install FB SDK component

```
react-native init lab2
npm install react-native-fbsdk@0.6.0 --save
react-native link react-native-fbsdk
```

### Configuration

#### Android

**Note** Please install `Android Studio` to manage Android project https://developer.android.com/studio/index.html

1. Open your Android project in Android Studio. The Android project folder is located at `[YourApp]/android`, where `[YourApp]` is the name of your application.
2. Open the `MainApplication.java` file. This file is located in the /[YourApp]/android/app/src/main/java/com/[YourApp]/ subfolder of your project.

![Image](http://i.imgur.com/PYU5vqE.png)

3. Add the following import statements to the list of imports near the top of the file.

```
import com.facebook.FacebookSdk;
import com.facebook.CallbackManager;
import com.facebook.appevents.AppEventsLogger;
```

4. Inside the `MainApplication class`, add an instance variable of type `CallbackManager`, and also add its getter.

```
public class MainApplication extends Application implements ReactApplication {

  private static CallbackManager mCallbackManager = CallbackManager.Factory.create();

  protected static CallbackManager getCallbackManager() {
    return mCallbackManager;
  }
  //...
```

5. Register the SDK package in the method `getPackages()`. Only add the indicated line.

```
 @Override
  protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        //**  ADD THE FOLLOWING LINE **//
        new FBSDKPackage(mCallbackManager)
    );
  }
```

6. Override `onCreate()` method

```
@Override
public void onCreate() {
  super.onCreate();
  FacebookSdk.sdkInitialize(getApplicationContext());
  // If you want to use AppEventsLogger to log events.
  AppEventsLogger.activateApp(this);
}
```

7. In `MainActivity.java`, add the following import statement.

```
import android.content.Intent;
```

8. Provide the ability to receive application events by overriding the `onActivityResult()` method.

@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    MainApplication.getCallbackManager().onActivityResult(requestCode, resultCode, data);
}

![Image](http://i.imgur.com/3ySneUj.png)

9. **This step is very important, this step will register application id with FBSKD**
  - Click `Create new App` https://developers.facebook.com/docs/facebook-login/android and keep this open b/c you will need `facebook_app_id` for your app. It's unique for each app.
  - Enter your android `Display Name` in the popup by your `ApplicationId`, you can find in this path `[YourApp]/android/app/build.gradle` and find `applicationId`
  
  ![Image](http://i.imgur.com/1IVHX89.png)
  
10. Add the Maven Central Repository to `build.gradle` before dependencies: `[YourApp]/android/build.gradle`

```
mavenCentral()
```

![Image](http://i.imgur.com/eZT4C14.png)

11. Add compile `'com.facebook.android:facebook-android-sdk:[4,5)'` to your `build.gradle` dependencies: `[YourApp]/android/app/build.gradle`

```
compile "com.facebook.android:facebook-android-sdk:[4,5)"
```

![Image](http://i.imgur.com/2usIYiA.png)

12. Edit Your `Manifest`, open your `strings.xml` file: `[YourApp]/app/src/main/res/values/strings.xml` 
**Note** Copy all xml tag, to get XML tag you go back to `Step 9`, open the the link to get `facebook_app_id`. This is an example I took from my FB

![Image](http://i.imgur.com/dnnMYpE.png)

13. Add a `meta-data` element to `AndroidManifest.xml`: `[YourApp]/android/app/src/main/AndroidManifest.xml`

```
<meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id"/>
```

![Image](http://i.imgur.com/EHsU8Gn.png)

14. The last step, sync gradle files to download FB dependencies, by click `Sync Now` or click on Gradle Icon on Toolbar: `Sync Project With Gradle Files`

![Image](http://i.imgur.com/9rFm49O.png)

15. Build you react native project again

```
react-native run-android
```

