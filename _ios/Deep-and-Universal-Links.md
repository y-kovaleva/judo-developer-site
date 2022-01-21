---
layout: page
title:  "Deep and Universal Links"
date:   2022-01-18 14:33:57 -0500
platform: iOS
step: 1
pageSection: "Getting Started"
---
# Deep and Universal Links

Experiences are hosted at and ultimately identifiable by a web link (a URL), sometimes referred to as universal links or app links, with associated domains. Such links are fully qualified web URLs with `https` schemes.

Judo can also be used with deep links, which are links with a custom scheme (a URI), and are not web links.  They are usually used for routing the user between different areas within the app, with less friction than might be involved with web links (avoiding prompts to the user open the web browser, a particular limitation on iOS), such as for in-app CMSes and push notifications. Note that if you have an existing deep link schema, there's a real possibility you can re-use it with Judo as well, because the Judo experience deep links will not conflict with your other pre-existing deep links.

As such, we support opening Judo Experience links with both `http` and `https` links as well as any custom URI scheme that you define (internally, we simply replace any scheme given to Judo `openURL` method with `https`).

## General Setup

You will have received a Judo universal link domain from us when setting up your account (such as `myapp.judo.app`, which will be used below as an example). You will need to set up your app to open such links, and then use `Judo.sharedInstance.openURL` to handle opening it.

For deep (custom scheme) links, you should decide on a URI scheme for them. The purpose of these "shim" deep links is to allow other parts of your app (such as a CMS) to link directly to Experiences without potentially displaying the user a prompt to open a browser.

Continue below for setting up these handlers using the standard iOS infrastructure.

### Configure Links in iOS Project Settings

#### Universal Links Setup

1. Add (or ensure you already have) the Associated Domain entitlement;
2. In the Associated Domains list, add your Judo domain you received from Judo (using the `applinks` prefix).

![Associated Domains Config](images/associated-domains-config.png)

**Note**: the server-side configuration is handled for you by the Judo server.
#### Deep Links Setup

Add (or ensure you already have) the URL type for the deep link scheme you selected (note that you did not receive this scheme from Judo; this is up to you):

![URL Types Config](images/url-types-config.png)

## Routing Inbound Links to the Judo SDK

At this stage, all that remains is to open the URIs with Judo.  Judo provides an `openURL()` helper that makes this easy. If Judo determines (by matching the domain name component of the URI) that the link is a Judo link, `openURL()` returns true and the Experience is opened.

### SceneDelegate

If your app is opted into the newer Scenes system, then you will want to integrate (merging as needed with any existing code) the needed link handlers into your Scene Delegate:

`willConnectTo` handler, needed for universal links (all cases) and deep links (launching from cold):

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    if let userActivity = connectionOptions.userActivities.first {
        Judo.sharedInstance.continueUserActivity(userActivity, animated: false)
    } else if let urlContext = connectionOptions.urlContexts.first {
        Judo.sharedInstance.openURL(urlContext.url, animated: false)
    }
}
```

`openURLContexts` handler, needed for handling deep links when app is already open:
```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
    if let context = URLContexts.first {
        Judo.sharedInstance.openURL(context.url, animated: true)
    }
}
```

### AppDelegate

If your app has not yet adopted Scenes, then you will want to integrate (merging as needed) the following link handlers into your Application Delegate:

For NSUserActivity handling:
```swift
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
    if userActivity.activityType == NSUserActivityTypeBrowsingWeb,
        let incomingURL = userActivity.webpageURL {
        return Judo.sharedInstance.openURL(incomingURL, animated: false)
    }
    return false
}
```

For Open URL handling:
```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return Judo.sharedInstance.openURL(url, animated: false)
}
```

## Further Reading

For Universal Links:

[Apple Developer - Supporting Universal Links in your App](https://developer.apple.com/documentation/xcode/allowing_apps_and_websites_to_link_to_your_content/supporting_universal_links_in_your_app)

For Deep Links:

[Apple Developer - Defining a Custom URL Scheme for Your App](https://developer.apple.com/documentation/xcode/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)

---

Continue to [Sync On Foregrounding](Sync-on-Foregrounding).
