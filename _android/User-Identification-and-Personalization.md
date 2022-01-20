---
layout: page
title:  "User Identification and Personalization"
date:   2022-01-18 14:33:57 -0500
category: Android
---
# User Identification and Personalization

You can identify your current user to Judo to enable personalization (of text copy and dynamic URLs on Actions, Images, WebViews, etc.) and also associate and attribute them in Judo's analytics.

## Identifying a User and setting Traits

On the `Judo` singleton, there is an `identify()` method you can call with an optional `userId` and a `traits`.

`userId` is a unique identifier for your user, constant across time. Don't use a user's email address here; that could potentially change if the user updates their email address in your app, for example.

`traits` is a `Map<String, Any>`, and is expected to be JSON-compatible data.

The `traits` dictionary is exposed as the `user.` data in the Experiences themselves.

```kotlin
Judo.identify(
    // NB: don't use an email address here!
    userId = "1234567",
    traits = hashMapOf(
        "premiumTier" to true,
        "email" to "john@example.com",
        "name" to "John Doe",
        "tags" to listOf("foo", "bar", "baz"),
        "pointsBalance" to "50000"
    )
)
```

This information is persisted across app restarts, but naturally we recommend calling it every time your user data changes (such as logging in, logging out, etc). Duplicate Identify events, provided they are not called to excess, are fine. Calling it every app start is a good idea, which will particularly help with the case where the user was previously logged in before you ship an update which includes the Judo integration.


## Usage within an Experience

The designer can then consume these values in several different ways using Judo's interpolation syntax, prefixed with `user.`. In a text field, they can use something like the following in a Text layer:

    Good morning, {{ user.name }}.

And a URL in an Image layer might be something like:

    http://myapi.example.com/v1/users/{{ user.id }}/avatar.jpg

NB. The `userID` you provided to Identify is mixed into the userInfo data as `userID` and is also available. If you set a value named `userID` in traits directly (not recommended), it will be shadowed.

## Resetting User Data/Logging Out

Call Judo.reset() to clear the User ID, traits, and cycle the Anonymous ID.