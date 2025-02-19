# PsIronSource
IronSource SDK Unreal Engine plugin.
This plugin provides a wrapper for IronSource rewarded video ad unit.

## Installation

Cocoapods is required. Clone the repository in `Plugins` directory and run `make`.

## Initial setup

First of all familiarize yourself with official IronSource SDK docs and integration guides for your platform, for this plugin is basically a wrapper for the SDK with little additional features.

Enable plugin in Project settings and fill in keys and ids in Engine ini file:
```
[/Script/PsIronSource.PsIronSourceSettings]
bIronSourceEnable=True
IronSourceIOSAppKey=example
IronSourceAndroidAppKey=example
AdMobIOSAppId=example
AdMobAndroidAppId=example
```

If needed, populate SKAdNetwork identifiers list directly in `PsIronSource_UPL_IOS.xml`:
```
<!-- SKAdNetworkId start -->
<setElement result="dSKAdNetworkItemDict_example" value="dict"/>
<addElement tag="$dSKAdNetworkItemDict_example" name="dSKAdNetworkIdentifierKey"/>
<setElement result="dSKAdNetworkId_example" value="string" text="example.skadnetwork"/>
<addElement tag="$dSKAdNetworkItemDict_example" name="dSKAdNetworkId_example"/>
<addElement tag="$" name="dSKAdNetworkItemDict_example"/>
<!-- SKAdNetworkId end -->
```

Alternatively you can use the provided `update-skadnetworks.py` Python script which will take network identifiers from `skadnetworks` file.

## Basic usage

After getting GDPR consent from user you can set consent status using `UPsIronSourceLibrary::GetIronSourceProxy()->SetGDPRConsent(bConsent)`.

Initizalize SDK using `UPsIronSourceLibrary::GetIronSourceProxy()->InitIronSource(UserId)` with user id as parameter.

You can get notifications about various ad-related events by binding to `VideoStateDelegate`: `UPsIronSourceLibrary::GetIronSourceProxy()->VideoStateDelegate.AddDynamic(this, &AExampleGameMode::OnVideoAdStateChanged)`.

## Rewarded video

To query rewarded video availability, use `HasRewardedVideo`, `IsRewardedVideoCappedForPlacement` or `VideoStateDelegate` events.

To start rewarded video, call `ShowRewardedVideo`.

## Interstitials

To query interstitial availability, use `IsInterstitialReady`, `IsInterstitialCappedForPlacement` or `VideoStateDelegate` events. 

To start interstitial, call `ShowInterstitial`. You should load interstitial manually with function `LoadInterstitial` before first call `ShowInterstitial` and every time after interstitial was shown, e.g., you can use events `InterstitialClosed`, `InterstitialShowFailed` and `InterstitialLoadFailed`.

## Offerwall

To query offerwall availability, use `HasOfferwall` or `OfferwallStateDelegate` events. 

To start offerwall, call `ShowOfferwall` or `ShowOfferwallWithPlacement`. You can get player's offerwall earn credits with function `GetOfferwallCredits`, it will trigger `EIronSourceOfferwallEventType::Credited` event.

To enable auto fire `EIronSourceOfferwallEventType::Credited` event, call function `SetOfferwallUseClientSideCallbacks (true)` before `InitIronSource`.