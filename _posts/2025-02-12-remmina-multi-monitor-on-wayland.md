---
title: 'How to Use Remmina Multi-Monitor on Wayland'
description: 'To use Remmina with multiple monitors, you must run it on Xwayland instead'
date: '2025-02-12 19:06:54 +0900'
last_modified_at: '2025-02-12 19:06:54 +0900'
layout: post
categories: [Linux]
tags: [remmina, rdp, linux, wayland, xwayland]
---

## Why Doesn't Remmina Work with Multiple Monitors?

Remmina was developed for X11 and works well on it.

Currently, Remmina's multi-monitor feature does not work on Wayland
[#2644](https://gitlab.com/Remmina/Remmina/-/issues/2644)
[#2686](https://gitlab.com/Remmina/Remmina/-/issues/2686).  

**To use Remmina with multiple monitors, you must run it on Xwayland instead.**

Note that Remmina is transitioning to GTK4
[PR#2465](https://gitlab.com/Remmina/Remmina/-/merge_requests/2465),
which involves many re-implementations, including the multi-monitor feature.

## Running Remmina on Xwayland  

Run the following command in your terminal:  

```bash
GDK_BACKEND=x11 remmina
```

## How to Enable Key Grabbing  

By default, Wayland does not enable Xwayland key grabbing.
You must manually enable it and add Remmina to its access rules list.

### Steps to Apply  

1. **Run Remmina on Xwayland** (see above).

2. **Identify the Xwayland window and get its name:**
   Run the following command in your terminal:

   ```bash
   xprop WM_CLASS
   ```

3. **Click the Remmina window running on Xwayland.**
   The output should look like this:

   ```text
   WM_CLASS(STRING) = "org.remmina.Remmina", "org.remmina.Remmina"
   ```

   Note this value *(in this case, `org.remmina.Remmina`)*.

   * *If nothing happens,* ensure that Remmina is running on Xwayland.
     If it is, you will see a `+` crosshair pointer when your cursor is over the Remmina window.

4. **Set the Xwayland grab access rules:**
   Run the following command in your terminal:

   ```bash
   # Replace org.remmina.Remmina with your actual value
   gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules "['org.remmina.Remmina']"
   ```

   * *Before doing this,* if you want to save your previous value, run:

     ```bash
     gsettings get org.gnome.mutter.wayland xwayland-grab-access-rules
     ```

5. **Enable Xwayland key grabbing:**
   Run the following command in your terminal:

   ```bash
   gsettings set org.gnome.mutter.wayland xwayland-allow-grabs true
   ```

   * *Before doing this,* if you want to save your previous value, run:

     ```bash
     gsettings get org.gnome.mutter.wayland xwayland-allow-grabs
     ```

### Steps to Revert

1. **Disable Xwayland key grabbing:**
   Run the following command in your terminal:

   ```bash
   gsettings set org.gnome.mutter.wayland xwayland-allow-grabs false
   ```

2. **Remove Xwayland grab access rules:**
   Run the following command in your terminal:

   ```bash
   gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules "[]"  # or restore your previous value
   ```

## What About Running Remmina with `sudo`?  

**Worst practice.**  

Even though it *seems* like running Remmina with `sudo` works fine on Wayland,
it may actually be running on Xwayland.
Normally, running an application in a terminal with root privileges results in it running on Xwayland instead of Wayland.

It just *seems* like it solves the issue, but it actually doesn't.
