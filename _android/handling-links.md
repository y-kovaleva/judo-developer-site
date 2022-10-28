---
layout: page
title:  "Handling Links"
platform: Android
step: 1
pageSection: "Getting Started"
---
<section id="{{ page.title | slugify }}" markdown=1>
# Handling Links

Experiences are hosted at and ultimately identifiable by a web link (a URL), sometimes referred to as universal links or app links, with associated domains. Such links are fully qualified web URLs with `https` schemes.

Judo can also be used with deep links, which are links with a custom scheme (a URI), and are not web links.  They are usually used for routing the user between different areas within the app, with less friction than might be involved with web links (avoiding prompts to the user open the web browser), such as for in-app CMSes and push notifications.

As such, we support opening Judo Experience links with both `http` and `https` links as well as a custom scheme that you define.
</section>
<section id="general-setup" markdown=1>
## General Setup

You will have received a Judo universal link domain from us when setting up your account (such as `myapp.judo.app`, which will be used below as an example).

For deep (custom scheme) links, you should decide on a URI scheme for them. The purpose of these "shim" deep links is to allow other parts of your app (such as a CMS) to link directly to Experiences without potentially displaying the user a prompt to open a browser.

For the standard setup of Judo, use an Intent Filter in your app's manifest to direct links to the Judo SDK's Experience Activity.

You can also readily adapt this to your own routing regime outside of Android's built in intent filter system (such as [branch.io](branch.io)).

### Configuring your Manifest

Add an `<activity>` element to your manifest for `app.judo.sdk.ui.ExperienceActivity`, then add an Intent filter that is responsible for handling both web (http/https scheme) and deep (custom scheme) links.

```xml
<!-- You'll need to add an Activity element to your manifest for Judo's ExperienceActivity
        (or your own subclass thereof to enable certain customizations) in order to specify the Intent filters -->
<activity android:name="app.judo.sdk.ui.ExperienceActivity" android:theme="@style/Theme.MaterialComponents.DayNight.NoActionBar">
    <!-- An Example Intent Filter that opens Experience links in Judo's ExperienceActivity -->
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />

        <!-- This Intent Filter can be activated from a web browser -->
        <category android:name="android.intent.category.BROWSABLE" />

        <!-- Include the Judo domain(s) you have configured -->
        <data android:host="brand1.judo.app" />

        <!-- Always include these two schemes for standard Experience links -->
        <!-- eg: https://brand1.judo.app/my-experience -->
        <data android:scheme="http" />
        <data android:scheme="https" />

        <!-- Lastly, you can also opt to use allow a custom scheme in your links (aka "deep links") -->
        <!-- eg: example://brand1.judo.app/my-experience -->
        <data android:scheme="example" />
    </intent-filter>

</activity>
```

### Configuring App Links Verification

In order to have web links to Experiences directly open in your app when clicked on in a browser, social media app, or email, without a "Choose App" prompt occurring, there's an additional step to attest to the Android operating system that your Judo domain belongs to you and your app.

The Judo cloud service directly generates and hosts the `.well-known/assetlinks.json` file, provided you provide us with the SHA256 fingerprint for the certificate you use to sign your APKs.

Log in to Judo's cloud interface, enter Settings, create an Android app if you do not already have one, and add the SHA Certificate Fingerprint.

Then add `android:autoVerify="true"` to any one of your Intent filters. Note that if any one of your configured app links in your entire manifest fails to validate, then none of them will.
</section>
<section id="further-reading" markdown=1>
## Futher Reading

[Android Developer Documentation - Handling Android App Links](https://developer.android.com/training/app-links)

[Android Developer Documentation - Verify Android App Links](https://developer.android.com/training/app-links/verify-site-associations)
</section>
