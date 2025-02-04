# SilentDragon Android - Android frontend for SilentDragon

SilentDragon Android is an Android frontend for the desktop SilentDragon that lets you send and receive shielded payments from your mobile phone.

<img height=50% width=50% src="https://raw.githubusercontent.com/MyHush/SilentDragonAndroid/master/SDA.jpg">

## When will it be in my favorite app store?

We are planning to submit to app stores Real Soon Now

### Running SilentDragon

In order to let your Android phone connect to your desktop, you need to run the deskop [SilentDragon](https://github.com/MyHush/SilentDragon), and sync fully. This is not a full node
on your Android (your poor battery!). It's a remote control for your full node.

Thankfully this should only take a short time with a fast internet connection!
As the Hush network grows, it will take longer. As of Sept 2019, the blockchain
is about 900MB on disk.

After your node is synced, go to `Apps -> Connect Mobile App` to view the
connection QR Code, which you can scan from the Android App.

## Dev Download

You can also head over to the [Releases page](https://github.com/MyHush/SilentDragonAndroid/releases) to download an APK. Please report bugs if you see any!

### Install the Android APK directly

If you're installing the APK directly, you'll need to allow `Install from untrusted sources` on your Android phone.

### Bugs???

You can file issues in the [issues tab](https://github.com/MyHush/SilentDragonAndroid/issues).

We appreciate them! Please follow the Github issue template, when reasonable.

### Compiling from source

On OS X:

    brew doctor
    brew install ant
    brew install maven
    brew install gradle
    brew cask install android-sdk
    brew cask install android-ndk

    touch ~/.android/repositories.cfg
    sdkmanager --update
    sdkmanager "platform-tools" "platforms;android-28"
    gradle build

On Debian-based systems:

    apt-get install -y android-sdk gradle
    touch ~/.android/repositories.cfg
    sdkmanager --update
    sdkmanager "platform-tools" "platforms;android-28"
    gradle build

Make sure you have Gradle 5.4.x or higher, 5.4.1 is known to work:

    ./gradlew wrapper --gradle-version=5.4.1

## Release Build Process

The first time you create a release build you'll need to create two files. The
release keystore is used for app signing and a properties file is used to store
sensitive information about the keystore. These files should not be committed
to git. Once you have both of these files you can create a release build for
the Google Play Store.

### Creating a release keystore via CLI

The `keytool` command can be used, for example:

    keytool -genkey -alias silentdragon -keyalg RSA -keystore new.jks -dname "CN=Duke Leto, O=Hush" -storepass testing -keypass 123

### Creating a release keystore via GUI

* With Android Studio IDE open, on the system bar click Build -> Generate Signed Bundle/APK
* Select the APK option instead of the Bundle option
* On the next screen select app as the module and click "Create new"
* Set the Key Store Name to `silent_dragon_keystore.jks` and the path to that of the project, create a password for the keystore path, a Key alias, and a key password. The store password and key password should be the same. Fill out some basic organization information and click Ok.
* On the next screen make sure the build variant "release" is selected and click Finish.

### Creating a properties file

Within the projects main directory create a file secrets.properties The
contents of the file are below and filled in with the release keystore alias,
key password, and store password from when you created the release keystore.

```
store_file_location=../silent_dragon_keystore.jks
key_alias=<alias>
key_password=<key_password>
store_password=<store_password>
```

### Building a release APK for Google Play

Before creating each build you should increment the version code & version name
in the build.gradle file. These must be incremented for each release otherwise
the Play Store will reject the build.

To create a release build navigate to the project directory in terminal and run

`./gradlew clean assembleRelease`

This will produce an apk file in the following directory.

    SilentDragonAndroid/app/build/output/apk/release/app-release.apk

This build can be directly uploaded to Google Play.

### Building a release APK for F-Droid

...
