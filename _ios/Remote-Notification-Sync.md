---
layout: page
title:  "Remote Notification Sync"
platform: iOS
step: 3
pageSection: "Getting Started"
---
<section id="remote-notification-sync">
{% capture text %}
# Remote Notification Sync

The Judo cloud service will send a remote APNs (Apple Push Notification service) notification to your app whenever a relevant Experience is added or updated. The SDK can receive these pushes and trigger its background sync process.
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="configure-an-apns-key">
{% capture text %}
## Configure an APNs Key

Note that Apple only allows a single APNs Key to be created in your organisation's vendor account.  Create an APNs Key at the Apple Developer portal's [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/authkeys/add) if you have not already. Then ensure it is added to your Judo account's [App Settings](https://www.judo.app/login).
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="configure-your-app-and-the-sdk-to-receive">
{% capture text %}
## Configure Your App and the SDK to Receive

Check the  "Remote notifications" checkbox in the Background Modes section of your app's target.

Add the following AppDelegate methods (or integrate with your existing ones):

Send the app's APNs push token to Judo:

```swift
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    Judo.sharedInstance.registeredForRemoteNotifications(deviceToken: deviceToken)
}
```

Receive incoming silent pushes:

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    // If the notification contents identify it as having been sent from the Judo cloud service, a sync will be triggered.
    Judo.sharedInstance.handleDidReceiveRemoteNotification(userInfo: userInfo) { result in
        completionHandler(result)
    }
}
```

Handle failure to register for remote notifications:

```swift
func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
    os_log("Failed to register for remote notifications, because: %@", type: .debug, error.localizedDescription)
}
```
{% endcapture %}
{{ text | markdownify }}
</section>