---
layout: page
title:  "Background Sync via WorkManager"
platform: Android
step: 3
pageSection: "Getting Started"
---
<section id="{{ page.title | slugify }}" markdown=1>
# Background Sync via WorkManager

Google's [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) library is the typical means that Android developers schedule and manage background tasks in a way that is compatible with a wide range of Android versions.

Here we provide some boilerplate for setting up a WorkManager job to run Judo's sync opportunistically in the background.
</section>
<section id="add-workmanager-dependency" markdown=1>
## Add WorkManager Dependency

Add the following the `dependencies` block in your build.gradle:

Groovy:

```groovy
implementation 'androidx.work:work-runtime:2.5.0'
```

Kotlin KTS:

```kotlin
implementation("androidx.work:work-runtime:2.5.0")
```
</section>
<section id="define-worker" markdown=1>
## Define Worker

Create a `PeriodicWorkRequest` that runs the Judo SDK's sync:

```kotlin
import androidx.work.*
import java.util.concurrent.CountDownLatch
import java.util.concurrent.TimeUnit
import app.judo.sdk.api.Judo

class JudoSyncWorker(appContext: Context, workerParameters: WorkerParameters): Worker(appContext, workerParameters) {
    companion object {
        private const val TAG = "JudoSyncWorker"
    }

    override fun doWork(): Result {
        val latch = CountDownLatch(1)

        Judo.performSync {
            latch.countDown()
        }

        try {
            latch.await()
        } catch (error: Exception) {
            Log.e(TAG, "Unable to complete background Judo sync")
            return Result.failure()
        }
        return Result.success()
    }
}
```

 ## Register Task

 Finally, in your Application class' `onCreate`, create and register a work request:

 ```kotlin
 val judoWorkRequest = PeriodicWorkRequest.Builder(JudoSyncWorker::class.java, 1, TimeUnit.HOURS)
    .setConstraints(
        Constraints.Builder()
            // some recommended constraints in order to be considerate about the user's battery and network.
            .setRequiredNetworkType(NetworkType.UNMETERED)
            .setRequiresCharging(true)
            .build(),
    )
    .build()

WorkManager.getInstance(this).enqueueUniquePeriodicWork("judo-sync", ExistingPeriodicWorkPolicy.REPLACE, judoWorkRequest)

 ```

---

You've completed the initial integration!  However, other optional steps remain for for more advanced features, namely [User Identification and Personalization](user-identification-and-personalization), handling both [Integrating with Analytics](integrating-with-analytics).
</section>