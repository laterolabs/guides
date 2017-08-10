# React-Native Lifecycle

#### Initialize App w/ Basic Configurations
 - Use `react-native init`, ignite, or expo.
 - add basic must-have packages: vector icons, animatable, config, keychain, push notifications, navigation, etc.
 - Consider offline first capabilities (even if just adding https://github.com/gnestor/react-native-statusbar-alert + NetInfo)
 - Lock orientation (if necessary)
 - Decide on minimal support (iPhone 4/5/6? Android?)
 - Configure custom permissions for Android in the `AndroidManifest.xml`
 - Configure custom permissions for iOS in Entitlements tab.
 - Add jest for testing
 - Add custom fonts (if any) to both Xcode and Android.
 - If API will *NOT* enforce HTTPS, disable `App Transport Security Settings` in Info.Plist for iOS.

#### iTunes Connect
 - Create necessary development and production certificates (push notification etc.) and provisioning profiles for the app.

#### Create App in Both Playstores
  - Create the app with a consistent name com.company.product etc.
  - Fill out all necessary information for each store, description, price, policies.
  - Add all app store assets: images/videos (tool for creating assets: http://apetools.webprofusion.com/tools/imagegorilla) OR https://github.com/bamlab/generator-rn-toolbox

#### Add Mobile Center/Crash Reporting tools
- Use Azure Mobile center, or Fastlane, or BuddyBuild, or Crashlytics, or CodePush whatever you need.
- Mobile center is recommended for its ease of setup and its extensive tools.

#### Additional Android steps:
- Follow the steps here: https://facebook.github.io/react-native/docs/signed-apk-android.html to generate a signed APK to add to your app so you can upload it to the play store.
