---
layout: post
title:  Xcode Tips for iOS Security Researcher
date:   2021-11-18
categories: xcode
---

### Reduce Disk Space
* Note: on your own risk

1. If don't do the watchOS related work, delete:
```
/Applications/Xcode.app/Contents/Developer/Platforms/WatchOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/watchOS.simruntime
```

2. If don't do the tvOS related work, delete:
```
/Applications/Xcode.app/Contents/Developer/Platforms/AppleTVOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/tvOS.simruntime
```

3. If only draw UI by programming, not using `storyboard` etc, delete(better keep it):
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime
```

4. do sim cleaning:
```
xcrun simctl delete unavailable
```

5. delete unused device support files:
```
~/Library/Developer/Xcode/iOS DeviceSupport
~/Library/Developer/Xcode/watchOS DeviceSupport
```

6. delete `DerivedData`:
```
~/Library/Developer/Xcode/DerivedData
```

7. delete `CoreSimulator` cache:
```
~/Library/Developer/CoreSimulator/Caches/dyld
```

### Multiple Versions
If don't care about the UI of Xcode, you don't need to install two or more Xcode:
1. copy the `XcodeDefault.xctoolchain` from the newer version to
```
/Applications/Xcode.app/Contents/Developer/Toolchains/
```
then rename it, and use it later.

2. copy the `SDK` to:
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs
/Applications/Xcode.app/Contents/Developer/Platforms/DriverKit.platform/Developer/SDKs
```
and choose the sdk in Xcode's project config pane.

3. copy the `DeviceSupport` to:
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
/Applications/Xcode.app/Contents/Developer/Platforms/AppleTVOS.platform/DeviceSupport
/Applications/Xcode.app/Contents/Developer/Platforms/WatchOS.platform/DeviceSupport
```

### PS
1. 
```
iOS developers: there's one more place to clean up if you're running Xcode 16: "/System/Library/AssetsV2/com_apple_MobileAsset_iOSSimulatorRuntime”
Although it's in /System, it's actually a separate mount point so it is read/write. You can delete older runtime folders in there that you're no longer using. This saved ~30GB on my Mac

https://mastodon.social/@_inside/112904004439494121
```