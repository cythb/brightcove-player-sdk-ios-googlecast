# Google Cast Plugin for Brightcove Player SDK for iOS, version 6.8.7.1605

Requirements
============

- Xcode 11.0+
- ARC

Supported Platforms
==========
iOS 11.0 and above.

Installation
==========
The Google Cast Plugin for Brightcove Player SDK provides a static library framework for installation.

The Google Cast plugin supports version 4.5.0 of the Google Cast SDK for iOS. The Google Cast release notes can be found [here](https://developers.google.com/cast/docs/release-notes).

CocoaPods
----------

You can use [CocoaPods][cocoapods] to add the Google Cast Plugin for Brightcove Player SDK to your project.  You can find the latest Brightcove-Player-GoogleCast podspecs [here][podspecs]. The pod will incorporate the correct version of GoogleCast automatically. CocoaPods 1.0 or newer is required.

CocoaPod Podfile example:

```bash
source 'https://github.com/CocoaPods/Specs'
source 'https://github.com/brightcove/BrightcoveSpecs.git'

platform :ios, '11.0'

use_frameworks!

target 'ExampleApp' do
    pod 'Brightcove-Player-GoogleCast'
end
```

Installing Brightcove-Player-GoogleCast automatically installs the dependency, Brightcove-Player-Core. Based on your needs, you may choose to include either the dynamic Brightcove-Player-Core framework, or the static one. By default, the dynamic Core framework is installed. To choose the static Core framework, your Podfile would look something like this:

```bash
source 'https://github.com/CocoaPods/Specs'
source 'https://github.com/brightcove/BrightcoveSpecs.git'

platform :ios, '11.0'

use_frameworks!

target 'ExampleApp' do
    pod 'Brightcove-Player-GoogleCast-static'
end
```

When updating your installation, it's a good idea to refresh the local copy of your BrightcoveSpecs repository to ensure you have the latest podspecs locally, just like you would update your CococaPods master repository. Use `pod repo update` to do so.

Manual
----------

To add the Google Cast Plugin for Brightcove Player SDK to your project manually:

1. Follow the [Google Cast SDK Manual Setup][googlecastsdkmanualsetup] guide.
1. Follow the [Brightcove Player SDK Manual Installation][bcovsdkmanualsetup] guide.
1. Download the [Google Cast Plugin for Brightcove Player SDK][bcovgooglecast] framework.
1. On the "General" tab of your application target, add BrightcoveGoogleCast.framework from the Google Cast Plugin for Brightcove Player SDK download to the list of **Frameworks, Libraries, Embedded Content**.
1. On the "Build Settings" tab of your application target, ensure that the "Framework Search Paths" include the paths to the frameworks. This should have been done automatically unless the framework is stored under a different root directory than your project.

Imports
----------
The Google Cast Plugin for Brightcove Player SDK can be imported into code a few different ways; `@import BrightcoveGoogleCast;`, `#import <BrightcoveGoogleCast/BrightcoveGoogleCast.h>` or `#import <BrightcoveGoogleCast/[specific class].h>`. You can import the `GoogleCast` and `BrightcovePlayerSDK` modules in similar fashion.

[bcovsdkmanualsetup]: https://github.com/brightcove/brightcove-player-sdk-ios#ManualInstallation
[googlecastsdkmanualsetup]: https://developers.google.com/cast/docs/ios_sender/#manual_setup 
[cocoapods]: http://cocoapods.org
[podspecs]: https://github.com/brightcove/BrightcoveSpecs/tree/master/Brightcove-Player-GoogleCast
[podspecs-static]: https://github.com/brightcove/BrightcoveSpecs/tree/master/Brightcove-Player-GoogleCast-Static
[release]: https://github.com/brightcove/brightcove-player-sdk-ios-google-cast/releases

Before You Begin
==========
Before attempting to utilize this plugin you should already be familiar with the following:

* [Register your application][registration]. This will walk you through creating your reciever application and grant you an application ID. Starting out you can use the demo application ID '4F8B3483'.
* [Setup for Developing with the Cast Application Framework (CAF) for iOS ][setupguide]. This will walk you through setting up the necessary entitlements for your app.
* [Initialze the Cast Context][castcontext]. This will walk you through initializing the Cast Context with your receiver application ID.
* [Add Mini Controllers][addminicontrollers]. This will walk you through how to take advantage of the Google Cast SDK's mini media controls. 

[registration]:https://developers.google.com/cast/docs/registration
[setupguide]:https://developers.google.com/cast/docs/ios_sender
[castcontext]:https://developers.google.com/cast/docs/ios_sender/integrate#initialize_the_cast_context
[addminicontrollers]:https://developers.google.com/cast/docs/ios_sender/integrate#add_mini_controllers

Quick Start
==========
The BrightcoveGoogleCast plugin is a bridge between [Google Cast iOS SDK][googlecast] and the [Brightcove Player SDK for iOS][bcovsdk]. 

This snippet shows its basic usage.

```
    [1] self.googleCastManager = [BCOVGoogleCastManager new];

    [2] id<BCOVPlaybackController> playbackController = [BCOVPlayerSDKManager.sharedManager createPlaybackController];

    [3] [playbackController addSessionConsumer:self.googleCastManager];
```

Breaking the code down into steps:

1. Create a reference to the BCOVGoogleCastManager singleton.
1. Create an instance of BCOVPlaybackController to use.
1. Add your BCOVGoogleCastManager reference as a session consumer to your BCOVPlaybackController instance.

[googlecast]: https://developers.google.com/cast/docs/ios_sender/
[bcovsdk]: https://github.com/brightcove/brightcove-player-sdk-ios
[bcovgooglecast]: https://github.com/brightcove/brightcove-player-sdk-ios-googlecast

Brightcove CAF Receiver
==========

The application ID for the Brightcove CAF Receiver is `341387A3` and is assigned to the constant `kBCOVCAFReceiverApplicationID`. You can verify the application ID by checking the [CAF Receiver config.json](https://players.brightcove.net/videojs-chromecast-receiver/2/config.json).

If you are using the Brightcove CAF Receiver you'll need to initialize these variables like this:

```
[1] BCOVReceiverAppConfig *receiverAppConfig = [BCOVReceiverAppConfig new];
    receiverAppConfig.accountId = kAccountID;
    receiverAppConfig.policyKey = kServicePolicyKey;

[2] self.googleCastManager = [[BCOVGoogleCastManager alloc] initForBrightcoveReceiverApp:receiverAppConfig];
```

The following properties are also available to set on `BCOVReceiverAppConfig` as needed:
* splashScreen (for customizing the splash screen image)
* playerUrl (for using a customized player)
* authToken (for use with PAS/EPA)
* adConfigId (for use with SSAI)
* userId (for use with analytics tracking)
* applicationId (for use with analytics tracking)

Delegate Methods
==========
BCOVGoogleCastManagerDelegate has four delegate methods that you can use to know when major casting-related events have occured. These are:

* `- (void)switchedToLocalPlayback:(NSTimeInterval)lastKnownStreamPosition` is called when a cast session ends.
* `- (void)switchedToRemotePlayback` is called when a cast session starts.
* `- (void)currentCastedVideoDidComplete` is called when a casted video has finished playing.
* `- (void)suitableSourceNotFound` is called when no suitable source is found to cast.

To take advantage of these events, simply set a delegate on the BCOVGoogleCastManager singleton. 

```
    self.googleCastManager.delegate = self;
```

Source Selection
==========
The BCOVGoogleCastManager will attempt to find a suitable source to use. It will look for sources in this order:

1. HTTPS HLS v3
1. HTTPS DASH
1. HTTPS MP4
1. HTTP HLS v3
1. HTTP DASH
1. HTTP MP4

If none of these are found, the delegate method `suitableSourceNotFound` will be called. 

Your Video Cloud account will need to be set up to support HLS v3, ensure that DASH is enabled or have an MP4 source available for each video.

Customizations
==========
There are two properties, in addition to the delegate property, that you can set on the BCOVGoogleCastManager class. These are:

* `GCKImage *fallbackPosterImage`: The GCKImage that will be used when there is no poster image available for a video.
* `CGSize posterImageSize`: The height and width that you want to use for the GCKImage object image that is created. Defaults to 480h x 720w.

Known Issues / Limitations
==========

## When using a default, unmodified receiver (including the demo receiver) the following limitations apply:

* DRM is not supported.
* Multiple Audio Tracks are not supported.
* Client-side and Server-side Advertising are not supported.
* Live and Live DVR streams are not supported.

## When using the Brightcove CAF receiver the following limitations apply:

* Client-side Advertising is not supported.

Support
=======
If you have questions, need help or want to provide feedback, please use the [Support Portal](https://supportportal.brightcove.com/s/login/) or contact your Account Manager.  To receive notification of new SDK software releases, subscribe to the Brightcove Native Player SDKs [Google Group](https://groups.google.com/g/brightcove-native-player-sdks).

