---
layout: page
title:  "Remote Notification Sync"
platform: Android
step: 2
pageSection: "Getting Started"
---
<section id="remote-notification-sync">
{% capture text %}
# Remote Notification Sync

The Judo cloud service will send a remote FCM (Firebase Cloud Messaging) push notification to your app whenever a relevant Experience is added or updated. The SDK can receive these pushes and trigger its background sync process. Note that this is not used for any user-facing push notifications whatsoever.
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="configure-fcm-in-your-app">
{% capture text %}
## Configure FCM in Your App

If you have not already integrated FCM into your app for your own push notification purposes, head to the [Firebase Console](https://console.firebase.google.com/) and ensure you are logged in with an appropriate Google account for your project. Select "Add a new project", or, if you have already created one, select it.

See Google's standard [Firebase Cloud Messaging Android Setup guide](https://firebase.google.com/docs/cloud-messaging/android/client).
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="configure-fcm-in-judo-cloud">
{% capture text %}
## Configure FCM in Judo Cloud

Enter FCM's Cloud Messaging tab, and copy the Server Key. Then ensure it is added to your an Android App in your Judo account's [App Settings](https://www.judo.app/login).
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="send-device-push-token-to-judo">
{% capture text %}
## Send Device Push Token to Judo

To send device tokens to Judo, implement `onNewToken` in your `FirebaseMessagingService`:

```kotlin
override fun onNewToken(p0: String) {
    super.onNewToken(p0)

    Judo.setPushToken(
        fcmToken = p0
    )
}
```
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="receive-judo-sync-request-notifications">
{% capture text %}
## Receive Judo Sync Request Notifications

To trigger a Judo sync on any incoming push notifications (that are marked as Judo notifications), implement `onNewToken` in your `FirebaseMessagingService`:

```kotlin
override fun onMessageReceived(p0: RemoteMessage) {
    super.onMessageReceived(p0)

    Judo.onFirebaseRemoteMessageReceived(data = p0.data)

}
```
{% endcapture %}
{{ text | markdownify }}
</section>