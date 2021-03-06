[![npm version](https://badge.fury.io/js/react-native-admob.svg)](https://badge.fury.io/js/react-native-admob)
## react-native-admob

A react-native module for Google AdMob GADBanner and GADInterstitial (react-native v0.19.0 or newer required).

The banner is implemented as a component while the interstitial has an imperative API.

### Installation

#### With [`rnpm`](https://github.com/rnpm/rnpm) (recommended)

1. `npm i react-native-admob -S`
2. `rnpm link`
3. Add [Google AdMob Framework](https://developers.google.com/admob/ios/quick-start#manually_using_the_sdk_download) to your Xcode project. This is only needed for iOS and guarantees your app is using the newest admob version.

#### Manual Installation

##### iOS

1. `npm i react-native-admob -S`
2. Add [Google AdMob Framework](https://developers.google.com/admob/ios/quick-start#manually_using_the_sdk_download) to your Xcode project.
3. Add react-native-admob static library to your Xcode project like explained [here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking). (Step 3 of this guide is not needed)

##### Android

1. `npm i react-native-admob -S`
2. Make the following additions to the given files:

**android/settings.gradle**

```
include ':RNAdMob', ':app'
project(':RNAdMob').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-admob/android')
```

**android/app/build.gradle**

```
dependencies {
   ...
   compile project(':RNAdMob')
}
```

**MainActivity.java**

On top, where imports are:
```java
import com.sbugert.rnadmob.RNAdMobPackage;
```

Under `protected List<ReactPackage> getPackages() {`:  
```java
  return Arrays.<ReactPackage>asList(
    new MainReactPackage(),
    new RNAdMobPackage()
  );
```

### Usage

```javascript
import { AdMobBanner, AdMobInterstitial } from 'react-native-admob'

// Display a banner
<AdMobBanner
  bannerSize="fullBanner"
  adUnitID="your-admob-unit-id"
  testDeviceID="EMULATOR"
  didFailToReceiveAdWithError={this.bannerError} />

// Display an interstitial
AdMobInterstitial.setAdUnitID('your-admob-unit-id');
AdMobInterstitial.setTestDeviceID('EMULATOR');
AdMobInterstitial.requestAd(AdMobInterstitial.showAd);
```

For a full example reference to the [example project](Example).


### Reference

#### AdMobBanner

##### bannerSize property
*Corresponding to [iOS framework banner size constants](https://developers.google.com/admob/ios/banner)*

| Prop value              | Description                                 | Size                  |
|-------------------------|---------------------------------------------|-----------------------|
|`banner`                 |Standard Banner for Phones and Tablets       |320x50                 |
|`largeBanner`            |Large Banner for Phones and Tablets          |320x100                |
|`mediumRectangle`        |IAB Medium Rectangle for Phones and Tablets  |300x250                |
|`fullBanner`             |IAB Full-Size Banner for Tablet              |468x60                 |
|`leaderboard`            |IAB Leaderboard for Tablets                  |728x90                 |
|**`smartBannerPortrait`**|Smart Banner for Phones and Tablets (default)|Screen width x 32|50|90|
|`smartBannerLandscape`   |Smart Banner for Phones and Tablets          |Screen width x 32|50|90|

*Note: There is no `smartBannerPortrait` and `smartBannerLandscape` on Android. Both prop values will map to `smartBanner`*


##### Events as function props
*Corresponding to [Ad lifecycle event callbacks](https://developers.google.com/admob/ios/banner)*

| Prop                                          |
|-----------------------------------------------|
|`adViewDidReceiveAd()`                         |
|`didFailToReceiveAdWithError(errorDescription)`|
|`adViewWillPresentScreen()`                    |
|`adViewWillDismissScreen()`                    |
|`adViewDidDismissScreen()`                     |
|`adViewWillLeaveApplication()`                 |

#### AdMobInterstitials

##### Methods

| Name                      | Description                                                                                                     |
|---------------------------|-----------------------------------------------------------------------------------------------------------------|
|`setAdUnitID(adUnitID)`    | sets the AdUnit ID for all future ad requests.                                                                  |
|`setTestDeviceID(deviceID)`| sets the test device ID                                                                                         |
|`requestAd(callback)`      | requests an interstitial and calls callback when `interstitialDidLoad` or`interstitialDidFailToLoad` event fires|
|`showAd(callback)`         | shows an interstitial if it is ready and calls callback when `interstitialDidOpen` event fires                  |
|`isReady(callback)`        | calls callback with boolean whether interstitial is ready to be shown                                           |

*For simulators/emulators you can use `'EMULATOR'` for the test device ID.*  
*Note: `tryShowNewInterstitial()` is deprecated as of v1.1.0 and can be replaced by calling `requestAd` with `showAd` as callback.*

##### Events
Unfortunately, events are not consistent across iOS and Android. To have one unified API, new event names are introduced for pairs that are roughly equivalent.

| iOS                                      | *this library*                   | Android             |
|------------------------------------------|----------------------------------|---------------------|
|`interstitialDidReceiveAd`                |`interstitialDidLoad`             |`onAdLoaded`         |
|`interstitial:didFailToReceiveAdWithError`|`interstitialDidFailToLoad`       |`onAdFailedToLoad`   |
|`interstitialWillPresentScreen`           |`interstitialDidOpen`             |`onAdOpened`         |
|`interstitialDidFailToPresentScreen`      |                                  |                     |
|`interstitialWillDismissScreen`           |                                  |                     |
|`interstitialDidDismissScreen`            |`interstitialDidClose`            |`onAdClosed`         |
|`interstitialWillLeaveApplication`        |`interstitialWillLeaveApplication`|`onAdLeftApplication`|

*Note that `interstitialWillLeaveApplication` and `onAdLeftApplication` are not exactly the same but share one event in this library.*


---

### TODO
- [ ] Support [Ad Targeting](https://developers.google.com/admob/ios/targeting)
- [ ] Also use interstitial event names for banner
