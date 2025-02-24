# Next steps for getting your PWA into the Google Play Store
You've successfully generated a Google Play Store app package for your PWA. 😎

Your next steps:
1. **Deploy `assetlinks.json` to your server** to prove domain ownership.
2. **Test your app**: open the `.apk` file on an Android device or Android emulator.
3. **Submit your app to Google Play**: upload the `.aab` file to the Google Play Store.
4. **Update `assetlinks.json` for production**: after Google Play signs your app, you'll need to update your asset links to remove the address bar from your app.

Each step is explained below.

## 1. Deploy `assetlinks.json`

Your zip file contains `assetlinks.json`. This is a [digital asset links file](https://developers.google.com/web/updates/2019/08/twas-quickstart#creating-your-asset-link-file) that proves ownership of your PWA. Upload this file to your server at `https://example.com/.well-known/assetlinks.json`. (Replace example.com with your PWA's URL.)

> 💁‍♂️ *Heads up*: 
> 
> **Digital asset links are required for your PWA to load without the browser address bar**. If you're seeing a browser address bar in your app on Android, your `assetlinks.json` file is missing, inaccessible, or incorrect. See our [asset links helper](/Asset-links.md) to fix this.

## 2. Test your app on an Android device or Android emulator
Your zip file contains an `.apk` (Android Package) file, which can be loaded on your personal Android device or Android emulator.

**To test your app, install open the `.apk` file on your Android device or emulator.**

If you don't have a physical Android device, you can use an Android emulator such as the free [Android Emulator included in Android Studio](https://developer.android.com/studio/run/emulator). Run the emulator and open the `.apk` file to install your app. You can also drag and drop the `.apk` file onto the Android emulator to install it.

## 3. Upload your app to the Google Play Store

Your zip file contains an `.aab` (Android App Bundle) file which can be submitted directly to the Play Store through the [Google Play Console](https://developer.android.com/distribute/console).

Once you submit your app, it will be reviewed. Once approved, your PWA will be available in the Google Play Store. 😎

## 4. Update your asset links file for production

> 💁🏽‍♀️ *Heads up*: 
> 
> If you skip this step, a browser address bar will appear in your app. See our [asset links helper](/Asset-links.md#validate-your-assetlinksjson-file) for more info.

Once you've uploaded your `.aab` file, Google Play re-signs your app. Because of this, you'll need to update your asset links file.

To update your asset links and remove the address bar from your app, login to the [Google Play Console](https://developer.android.com/distribute/console), select your app, then choose `Setup` -> `App integrity`, then copy your `SHA-256 fingerprint`:

<img src="/static/google-play-signing.png" width="600px" />

Then, paste the fingerprint into your `assetlinks.json` file:

```json
[
    {
        "relation": ...,
        "target": {
            "namespace": ...,
            "package_name": ...,
            "sha256_cert_fingerprints": [
                "...",
                "PASTE YOUR NEW SHA-256 FINGERPRINT HERE"
            ]
        }
    }
]
```

Once your updated `assetlinks.json` file is deployed to your server, the address bar will disappear from your app.

## Save your signing key

Your zip file contains `signing.keystore` and `signing-key-info.txt`:

- `signing.keystore` is the Android key store file containing the signing key.
- `signing-key-info.txt` is a text file containing your signing key information, such as the key password, store password, and key alias.

Keep both of these files in a safe place. **You'll need them to deploy future versions of your app.** See [Uploading a new version](#uploading-a-new-version) for more info.

## Uploading a new version

Have an existing app in the Play Store and want to update it to a new version? No problem! Just generate an Android package with a new version and with your existing signing key:

1. Go to [PWABuilder](https://www.pwabuilder.com) and input your PWA's URL
2. When analysis completes, click `Build My Package`
3. Choose `Android`, then click `Options`: <br><img src="/static/android-options.png" />
4. Specify your new `App version` and `App version code`:
<br><img src="/static/android-options-versions.png" />
5. Scroll down to `Signing key` and choose `Use mine`: <br><img src="/static/android-options-existing-signing-key.png" />
6. Choose your existing signing key file, and fill in your existing signing key information (`key alias`, `key password`, `store password`)
7. Build your package.

PWABuilder will build a package signed with your existing key. When you upload it to Google Play, it'll automatically be recognized as a new version of your existing app. 😎

## Note about Quality Criteria on Android

As of Chrome 86, PWAs downloaded from the Google Play Store will now crash if your app:
- Does not have a valid TLS certificate
- Does not work offline
- Does not link to your digital assetlinks file correctly

Because of this you should ensure that your PWA works completely offline, runs on an HTTPS domain and has your assetlinks file linked. For the assetlinks file, please refer to [Step 4 above](#4-update-your-asset-links-file-for-production). For your offline experience, we recommend running your PWA through [PWABuilder](https://pwabuilder.com) to ensure that your PWA works offline. If you don't have a service worker or need one that makes your app available offline, we recommend [choosing one here](https://www.pwabuilder.com/serviceworker). For more info about Chrome's quality criteria policy, check out [this article](https://blog.chromium.org/2020/06/changes-to-quality-criteria-for-pwas.html) from our friends over on the Chrome team.


## Need more help?

If the browser address bar is showing up in your app, see our [asset links helper](/Asset-links.md).

If you're otherwise stuck, we're here to help. You can [open an issue](https://github.com/pwa-builder/pwabuilder/issues) and we'll help walk you through it.
