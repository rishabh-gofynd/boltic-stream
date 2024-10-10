# Fynd Analytics Android (Segment Fork)

_Fynd Analytics_ provides invaluable insights into your user engagement, product metrics, and business performance. Harness the power of analytics to uncover vital details about your application's funnel, core business benchmarks, and ascertain product-market fit.

Powered by [Segment](https://segment.com), _Fynd Analytics_ offers a streamlined approach to collecting analytics data. With Segment, you can effortlessly relay this data to over 250 applications (including Google Analytics, Mixpanel, Optimizely, Facebook Ads, Slack, and Sentry). All it requires is a single Segment code snippet. Then, enable or disable integrations with a simple toggle—no extra coding necessary.

## Official Segment Documentation

Detailed usage instructions can be accessed on the official Segment platform: [Segment Mobile Android Documentation](https://segment.com/docs/sources/mobile/android/).

## Modifications: A Brief Overview

Several integral modifications were made to bring this repository up to speed:

1. **Method of Repository Creation**: Previously, the code was merely copied instead of being forked. This made it unfeasible to perform a diff with the official repository.
2. **Alterations in Package Name**: A change in the package name had led to modifications in all the files, obfuscating any distinct differences.
3. **Restrictions in Modifications**: The codebase's initial setup suggested that possibly only its initial modifier might amend it.

### How We Improved

To address the above challenges, the following corrective measures were implemented:

- Purged the existing codebase.
- Forked the official repository, ensuring no-push permissions.
- Excised superfluous files and tailored the code to meet our bespoke requirements.
- All event tracking is now redirected either to Boltic.io or a specified custom host.

### A Word on Package Naming

We opted against altering the package name. The prevailing MIT license affords us the freedom to adapt and employ the codebase across varied scenarios as long as there's no commercial undertone. Future updates might be encumbered by a change in package name.

## Synchronizing with the Official Segment Repository

To ensure your repository stays updated with the official `segment` repository, follow our detailed guide:

### 1. Fetch the Latest Updates

Begin by fetching the most recent data from both the `tracks` and `segment` repositories:

```bash
git fetch tracks
git fetch segment
```

### 2. Switch to Your Release Branch

Navigate to the branch you wish to update, for instance

```bash
git checkout release/x.y.z
```

### 3. Initiate the Rebase

Commence the rebase of your branch onto the master branch of the segment repository

```bash
git rebase segment/master
```

### 4. Address Any Conflicts

Should you encounter merge conflicts, address them:

1. Review the conflicted files and manually resolve the differences.
2. Post-resolution, stage the file:

```bash
git add .
```

3. With all conflicts resolved and files staged, continue:

```bash
git rebase --continue
```

Repeat this process until the rebase is finalized.

### 5. Upload Your Modifications

Post-rebase, relay the updated branch to your tracks repository. Due to the reshaped commit history, a force push is mandatory

```bash
git push tracks release/x.y.z --force-with-lease
```

## Key Considerations

- **Safety First**: Before embarking on the rebase, create a backup:

```bash
git branch backup-branch-name
```

- **Test, Test, Test**: Post-rebase, it's imperative to rigorously test the project. Ensure your changes align seamlessly with the recent segment updates.

- **Commit History**: Rebasing alters commit histories. Alert your collaborators as they'll need to refresh their local branches accordingly.

Armed with the prowess of git rebase, updating repositories becomes a cinch. Practice makes perfect—use it wisely!

## Fynd Android Analytics SDK Integration

Follow the steps below to integrate the Fynd Android Analytics SDK into your project. You also can refer Official Segment Documentation. Package name is also same as segment.

### 1. Clone your main Android project

```bash
git clone [your-android-project-url]
cd [your-android-project-root-directory]
```

### 2. Add the tracks repository as a submodule

```bash
git submodule add https://gitlab.com/fynd/regrowth/frolic/frontend/apps/android/tracks.git tracks
```

### 3. Initialize, update and add the submodule

```bash
git submodule init
git submodule update
git add .gitmodules tracks
git commit -m "Add tracks submodule"
```

### 4. Add the following line to your root settings.gradle file

```bash
include ':tracks'
```

### 5. Add the following line to the dependencies section of your app/build.gradle file

```bash
implementation project(path: ':track')
```

### 6. Initializing the SDK

```bash
private val analytics = Analytics.Builder(context, accessKey)
    .defaultApiHost(apiHost)
    .collectDeviceId(true)
    .logLevel(if (debugging)
        Analytics.LogLevel.VERBOSE
      else
        Analytics.LogLevel.NONE)
    .trackDeepLinks()
    .crypto(Crypto.none())
    .trackApplicationLifecycleEvents()
    .tag(System.currentTimeMillis().toString()).build()

Analytics.setSingletonInstance(analytics)
```

### 7. Tracking Events

Make sure your are not running this in main or ui thread.

```bash
val page = mapOf("title" to pageTitle, "url" to pageUrl)
val mainContext = Options().putContext("page", page)
analytics.track(event, Properties().apply { putAll(key-values) }, mainContext)
```

## Additional Configuration

If you wish to define another host without http or https. Ensure your server supports the your_url?accessKey=access authentication.

```bash
private val analytics = Analytics.Builder(context, accessKey)
    .defaultApiHost(SOME_URL_WITHOUT_HTTP_OR_HTTPS)
    .collectDeviceId(true)
    .logLevel(if (debugging)
        Analytics.LogLevel.VERBOSE
      else
        Analytics.LogLevel.NONE)
    .trackDeepLinks()
    .crypto(Crypto.none())
    .trackApplicationLifecycleEvents()
    .tag(System.currentTimeMillis().toString()).build()
```

------------------------------------------------------------------
