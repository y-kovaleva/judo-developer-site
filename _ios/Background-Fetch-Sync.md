---
layout: page
title:  "Sync via iOS Background Fetch"
platform: iOS
step: 4
pageSection: "Getting Started"
---
<section id="sync-via-ios-background-fetch">
{% capture text %}
# Sync via iOS Background Fetch

 iOS allow apps to register for the iOS to opportunistically call them to perform sync when network and power are at hand.  Judo includes a [BGTask](https://developer.apple.com/documentation/backgroundtasks/bgtask) that you can use to easily register Judo as a background task.
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="configure-capability">
{% capture text %}
## Configure Capability

 In the "Signing and Capabilities" in your Target, you need to select the "Background fetch" under Background Modes:

 ![Background Modes Editor]({{ 'assets/img/background-fetch-capability.png' | relative_url }})
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="add-permitted-identifier">
{% capture text %}
## Add Permitted Identifier

 In your `Info.plist`, you need to tell iOS that you are explicitly allowing the Judo SDK to register a background fetch task.
 
 First add the `BGTaskSchedulerPermittedIdentifiers` key as an Array to your `Info.plist`, which is rendered by Xcode with the friendly name of "Permitted background task scheduler identifiers".
 
 Select an identifiable string for it - we recommend `app.judo.background.refresh` - and add it to the Array. You can either use Xcode's plist editor directly, or "Custom iOS Target Properties" in the Info section of the Target settings.

![Background Fetch Permitted Identifier in Info.plist]({{ 'assets/img/background-fetch-permitted-identifier.png' | relative_url }})
{% endcapture %}
{{ text | markdownify }}
</section>
<section id="register-task">
{% capture text %}
## Register Task

 Finally, in your AppDelegate, call the SDK's `registerAppRefreshTask` helper method to register Judo's BGTask for you, using the identifier you selected above.

```swift
Judo.sharedInstance.registerAppRefreshTask(taskIdentifier: "app.judo.background.refresh")
```

---

You've completed the initial integration!  However, other optional steps remain for for more advanced features, namely [User Identification and Personalization](user-identification-and-personalization), handling both [Integrating with Analytics](integrating-with-analytics).
{% endcapture %}
{{ text | markdownify }}
</section>