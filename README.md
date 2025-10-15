# SlayTheSpireSaveExportableApk

Demonstrates an approach to play STS on Android device, while have the capability to export/import save files.

The game save files are at `/sdcard/Android/data/com.humble.SlayTheSpire/files/...`

Normally, when you use Android versions like 15, 16, you cannot access `/sdcard/Android/data`, so you need adb shell / Shizuku to access that path.

The special thing about SlayTheSpire is, its save files have different privileges, so you can't access/modify them even when you have adb shell.

And Android devices are more difficult to root these days.

So, the basic idea here is, create an APK that is wrapped, let it do the privileged parts for us.

Steps are like:
1. `apktool d slay.apk -o slay_mod`
2. Find the app launcher activity, `MainActivity.smali` or in my case, it's NativeActivity. (See `AndroidManifest.xml`, the launcher activity should have something like `<category android:name="android.intent.category.LAUNCHER"/>`)
3. If it's NativeActivity, we need to create a wrapper activity for it, and edit the `AndroidManifest.xml` accordingly to make the app start from our `MainActivity.smali`(See the files in release); If there is already an `MainActivity.smali`, just add the codes before its onCreate returns.
4. `apktool b slay_mod -o slay_wrapped.apk`
5. Transfer the `slay_wrapped.apk` to your device, use ApkTool M to sign and install.

If you can't understand the former part, just ask claude to make it more detailed for you. I made him wrote for me anyway.
