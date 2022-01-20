---
layout: page
title:  "Installation and Initialization"
date:   2022-01-18 14:33:57 -0500
category: iOS
step: 0
---
# Installation and Initialization

The Judo SDK allows you to present Experiences in your iOS mobile app. It supports using push-driven automatic sync in order to ensure that Experiences are available instantly (and when offline).

While the SDK may be linked with apps that have a target (minimum iOS SDK API level) of iOS 11, Judo content can only be displayed on iOS 13 and greater.

You need your app's Judo Access Token (found at the [Judo web interface](https://www.judo.app/login)).  You will need to configure (if your team has not done so already) an iOS app in the Judo settings. Once you have done so, there you will find the app's Access Token.

You will also a Domain configured in the Judo settings as well. These domains are subdomains within the `.judo.app` top-level domain.

## Install the SDK

The recommended way to install the Judo SDK is with SwiftPM.

To add the dependency to your project using Xcode (12 or later assumed), choose File -> Swift Packages -> Add Package Dependency, and type the URL to this repository into the search field:

    https://github.com/judoapp/judo-ios

In this next prompt, you will be able to choose the default SwiftPM update policy. The Judo SDK uses the standard semver versioning strategy, so the default of "up to next major" is recommended.  Leave the settings at their defaults and press Next.

It will prompt you to select which Judo products to add to your app target.  Make sure you select both JudoModel and JudoSDK, and press Next.

Once you have completed the SwiftPM installation flow, the SDK has been installed, and what remains now to is to complete configuring and integrating it.

## Initialize the SDK

For the next steps you will need your Access Token (discussed above) and an App Domain. Note that Judo supports multiple domains.

In your AppDelegate's `didFinishLaunchingWithOptions` template method, add the following lines (note you will need `import JudoSDK` to import the SDK's main module):

```swift
// Initialize the Judo SDK itself.
Judo.initialize(accessToken: "<ACCESS TOKEN HERE>", domain: "<APP-DOMAIN-HERE>")
// Request that Judo perform a sync on start. This is asynchronous, and in the event of no pending updates will consist of only a small, single HTTP request.
```

## Troubleshooting

If the `JUDO_VERBOSE` environment variable is set to 1 (you can do so in your Target's Scheme in Xcode), the Judo SDK will log additional information about its behaviour.

## Getting Help

See the [Judo Forum](https://forum.judo.app/c/sdk-integration).

---

Continue to [Deep and Universal Links](Deep-and-Universal-Links).

