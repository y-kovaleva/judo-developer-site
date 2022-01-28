---
layout: page
title:  "Sync on Foregrounding"
platform: iOS
step: 2
pageSection: "Getting Started"
---
The next step is to set up a bit of boilerplate to have the Judo SDK opportunistically sync content to the user's device when the app is brought to the foreground.

Depending on whether or not you have adopted Scenes in your app, choose the appropriate option from the following.

## Scene Delegate

If you have adopted Scenes, implement the `sceneDidBecomeActive(_)` template method in your App Delegate and have it call Judo's sync method:

```swift
func sceneDidBecomeActive(_ scene: UIScene) {
    Judo.sharedInstance.performSync()
}
```

## App Delegate

If you have not yet adopted Scenes, implement the `applicationDidBecomeActive(_)` template method in your Scene Delegate and have it call Judo's sync method:

```swift
func applicationDidBecomeActive(_ application: UIApplication) {
    Judo.sharedInstance.performSync()
}
```