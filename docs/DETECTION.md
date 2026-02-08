# BShield Detection in Android

This document lists all the detections observed in BShield for Android. The information is accurate as of Feburary 8th, 2026. If you discover additional detections, feel free to report them in the Issues tab.

> [!CAUTION]
> **This project is for educational purposes only. The intention is to highlight the weaknesses of current security solutions and to encourage the development of better, more reliable alternatives. Use this information responsibly. Do NOT use this for malicious intent. I am not responsible for the actions taken by users of this module or project.**

**Table of contents:**

- [BShield Detection in Android](#bshield-detection-in-android)
  - [Error Code 1 (Modified App Detection)](#error-code-1-modified-app-detection)
  - [Error Code 2 (Vitural Machine Detection)](#error-code-2-vitural-machine-detection)
  - [Error Code 3 (App List Detection)](#error-code-3-app-list-detection)
  - [Error Code 4 (Debug Tool Detection, Rare)](#error-code-4-debug-tool-detection-rare)
  - [Error Code 5 (Root Detection)](#error-code-5-root-detection)
    - [System properties](#system-properties)
    - [Maps detection](#maps-detection)
    - [Enforcing status](#enforcing-status)
    - [Package name detection](#package-name-detection)
    - [Leaks from custom launchers](#leaks-from-custom-launchers)
    - [Bootloader check](#bootloader-check)
    - [Suspicious mount](#suspicious-mount)
    - [\[UNCONFIRMED\] KSU/AP module image proc loop detection](#unconfirmed-ksuap-module-image-proc-loop-detection)
  - [Error Code 6 (Unlocked Bootloader Detection, Unused)](#error-code-6-unlocked-bootloader-detection-unused)
  - [Error Code 7 (App Detection, Rare)](#error-code-7-app-detection-rare)
  - [Error Code 8 (Privacy Space Like App Detection)](#error-code-8-privacy-space-like-app-detection)
  - [Error Code 10 (ADB Debug Mode Detection)](#error-code-10-adb-debug-mode-detection)
  - [Error Code 11 (Developer Mode Detection)](#error-code-11-developer-mode-detection)

## Error Code 1 (Modified App Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=1>

This error occurs when you install unsigned app or modified app. For patch app developers, currently I can't find any ways to make it working. So you can try, but it will be tough to make it working.

**Workaround:** Remove the modified, unsigned app from your system and install from Google Play.

## Error Code 2 (Vitural Machine Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=2>

This error occurs when you install the app in the vitural machine.

**Workaround:** Don't install the app in the vitural machine (Obviously :v).

## Error Code 3 (App List Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=3>

This error occurs when you install root manager, suspicious app in your device.

Below is the list of apps that BShield currently detects (there may be more; these are only the ones confirmed through testing. Feel free to request updates in the Issues tab):

```txt
com.topjohnwu.magisk
com.drdisagree.iconify
<most of lsposed app>
```

**Workarounds:**
You can use a combination such as:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

to hide these apps.

Or if you don't use root, just don't install the root manager app in your device.

## Error Code 4 (Debug Tool Detection, Rare)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=4>

This error occurs when you use google's debug tools to run. This won't happen in the production build of the app. If you experience this error, please contact the app developers or let me know.

## Error Code 5 (Root Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=5>

This is the toughest detection in BSheild when you're using root, it contains a variety of root, system detection. I will list some confirmed detection here:

### System properties

BShield also detects certain Android system properties. Some known examples include:

- `init.svc.adb_root`
- `service.adb.root`

**Workaround:**  
These properties can be hidden easily by overriding them, for example:

```sh
resetprop -n -p init.svc.adb_root ""
resetprop -n -p service.adb.root ""
```

**Note:** These properties will reset on reboot. You can use my example module in Release tab as a fix for every reboot

### Maps detection

BShield can also detect whether the memory maps contain traces of injection-related entries.  

You can verify this using the **Native Detector** tool ([download](https://github.com/reveny/Android-Native-Root-Detector/releases/latest)).

For example, it may report "Injection Detection".

**Here is some Workaround:**

- If your kernel supports KernelSU + SuSFS (with SUS_MAP enabled), you can add the leaked map paths to the SuSFS map list.
- If you're using font module, it may also leak map entries. Remove it or add its paths to SUS_MAP as mentioned above.
- You can also try Pedro's TreatWheel module to hide maps, but its effectiveness is limited and it requires a ReZygisk build to operate.

**A note about /system/framework/framework-res.apk maps detection.**

You may notice that in the **Native Detector** tool, it shows **Found Injection**, and the results look something like this:

```txt
7982fef000-7983031000 r--s 00000000 fd:00 1549 /system/framework/framework-res.apk
7988432000-7988448000 r--s 00344000 fd:01 1631 /system/framework/framework-res.apk
7988538000-7988546000 r--s 00000000 fd:03 3165 /system/framework/framework-res.apk
798bf3d000-798bf3e000 r--s 00055000 fd:00 1549 /system/framework/framework-res.apk
798bf3e000-798bf3f000 r--s 00000000 fd:03 3166 /system/framework/framework-res.apk
798bf4d000-798bf4e000 r--s 00002000 fd:03 3166 /system/framework/framework-res.apk
798bf4e000-798bf4f000 r--s 00000000 fd:27 518  /system/framework/framework-res.apk
7992736000-799273c000 r--s 0035d000 fd:01 1631 /system/framework/framework-res.apk
7992826000-7992827000 r--s 00000000 fd:27 503  /system/framework/framework-res.apk
7992976000-799297c000 r--s 00013000 fd:03 3165 /system/framework/framework-res.apk
79929ef000-79929f0000 r--s 00000000 fd:27 500  /system/framework/framework-res.apk
```

This happens because your custom kernel likely contains the LineageOS file-hiding patch in task_mmu.c. See: [reference commit (MoonWake@bea4fe4)](https://github.com/RainyXeon/moonwake_kernel_xiaomi_ruby/commit/bea4fe4ecfa41edb52f26ce9254a16643dda57ea).

The purpose of this LineageOS file-hiding patch is to replace real LineageOS file paths with `framework-res.apk`.

Basically, if a file mapped by a VMA contains lineage in its filename, then `/proc/<pid>/map_files/<start>-<end>` will point to `framework-res.apk` instead of the actual file. This prevents tools like MagiskDetector, root-checkers, app integrity scanners, MDM systems, etc., from detecting LineageOS files in memory maps.

The main idea for this LineageOS file hiding commit is to replace real file paths from LineageOS with `framework-res.apk`. Basically, if the file mapped by a VMA contains `lineage` in its filename, Then `/proc/<pid>/map_files/<start>-<end>` will point to `framework-res.apk`, not the real file. Tools like MagiskDetector, root-checkers, app integrity scanners, MDM systems, etc. cannot see LineageOS files in memory maps.

However, this hiding mechanism is outdated and unintentionally triggers **Found Injection** in **Native Detector**, because the fake VMA header is still detectable. This happens because the patch only replaces the file path, not the entire VMA metadata before that path (which is likely why BShield is able to detect it, in my opinion).

If you are a custom kernel developer, you can revert the commit that contains the LineageOS file-hiding code mentioned above. If you are a user, there is nothing you can do unless you replace the kernel or ask the developer to do so.

### Enforcing status

This is a common detection used by many applications. It is strongly recommended **not** to use a custom ROM with **permissive SELinux**, as it is considered insecure by modern standards.

If your ROM is running with **permissive SELinux**, certain attacks may be possible. BShield requires **SELinux** to be set to **Enforced** to function properly.

**Workaround:**

- Set SELinux to **Enforcing**

```sh
setenforce 1
```

- Use a kernel or ROM with **Enforcing SELinux**

### Package name detection

Another classic detection used by many applications, BShield checks the installed app list to identify apps commonly associated with root access. The original detection is from [error code 3](#error-code-3-app-list-detection), but for some reason, app like `me.bmax.apatch` or `com.rifsxd.ksunext` will trigger code 5 not 3. I believe this is the temporary Workaround of them to detect advanced root solution like KernelSU

Below is the list of apps that BShield currently detects (there may be more; these are only the ones confirmed through testing. Feel free to request updates in the Issues tab):

```txt
com.topjohnwu.magisk
com.drdisagree.iconify
com.rifsxd.ksunext
me.bmax.apatch
me.weishu.kernelsu
```

**Workaround:**
You can use a combination such as:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

to hide these apps.

### Leaks from custom launchers

BShield can detect many custom launcher modules, possibly through mounts, memory maps, or other indicators.

**Workaround:**  
The simplest approach is to remove custom launchers and use the default system launcher. Alternatively, using standard app launchers typically does not trigger detection.

### Bootloader check

I can confirm that Bshield is checking bootloader in the early 2026. Note is it only check of locking status and aosp keybox, revoked attestation key can still usable for now.

**Workaround:**
Install [JingMatrix/TEESimulator](https://github.com/JingMatrix/TEESimulator), put the package name into target.txt file like this:

```txt
com.vnid
```

### Suspicious mount

For a long time, BShield check for mount as a way to detect root. This happen when you install some modules like font changing or launcher.

You can verify this using the **Native Detector** tool ([download](https://github.com/reveny/Android-Native-Root-Detector/releases/latest)).

For example, it may report "Suspicious Mount".

**Workaround:**

- If you check your module zip file contains `mount --bind`, be careful, it may trigger this error code, ask your dev to [use this method instead](https://kernelsu.org/guide/module.html).
- KSU 3.0 and some recent version of APatch, Magisk can handle this carefully so you don't meet this detection most of the time.
- In some device, ReZygisk can't unmount some path weirdly. Tell me if you still meet that issue!

### [UNCONFIRMED] KSU/AP module image proc loop detection

In the recent reports from [@Hzzmonet](https://t.me/Hzzmonet), BShield also detect if the KSU/AP module image proc loop. This because in older KSU/AP, it use OverlayFS to operate, which cause detection.

You can verify this using the **Native Detector** tool ([download](https://dl.reveny.me/)).

For example, it may report "KSU/AP loop" or something similar like that.

**Workaround:**

- If you're in original or older KernelSU, please use Pedro's TreatWheel module to hide those.
- If you're in KernelSU-Next, please disable the `Use OverlayFS` switch in settings tab. You have to backup your module before operate.

## Error Code 6 (Unlocked Bootloader Detection, Unused)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=6>

This error occurs when you have unlocked bootloader. This kind of deteciton is not avaliable in most of BShield powered app. But if it's avaliable in the future, may see the Workaround below.

**Workaround:**

Install [JingMatrix/TEESimulator](https://github.com/JingMatrix/TEESimulator), put the package name into target.txt file like this:

```txt
com.vnid
```

## Error Code 7 (App Detection, Rare)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=7>

This error occurs when you have some suspicious app in your device. We don't experience this detection and it's rare to see that.

**Workaround:** same as [error code 3](#error-code-3-app-list-detection).

## Error Code 8 (Privacy Space Like App Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=8>

This error occurs when you use the app inside an emulator or privacy space like app.

**Workaround:** don't use the app inside a third party app

## Error Code 10 (ADB Debug Mode Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=10>

This error occurs when you use ADB debug mode in your device.

**Workarounds:**

Safe practice, don't enable ADB debug mode when you don't use it.

## Error Code 11 (Developer Mode Detection)

**Reference link:** <https://vneid.gov.vn/shield-warning?code=11>

This error occurs when you use Developer Mode in your device.

**Workarounds:**

You can use a combination such as:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [ImNotADeveloper](https://github.com/notyour777/ImNotADeveloper)

to hide developer mode, ADB debug mode

Or better practice, don't enable Developer Mode when you don't use it.
