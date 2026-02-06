---
title: Change Google Drive localized names to English
author: Ji Huang
date: '2023-06-04'
categories:
  - misc
tags:
  - google
meta_img: images/image.png
description: Description for the page
---

By default, Google Drive localized name will be the same as default. For example, on my laptop which has Windows-中文, Google Drive shows "我的云端硬盘". This created some errors for the file path in RStudio.

Here is how I changed it to English. The source is from Google help page: [English](https://support.google.com/a/answer/7644837?hl=en&sjid=4591694863418410542-NA), [中文](https://support.google.com/a/answer/7644837?hl=zh-Hans). I only did on the Windows machine.

1.  Open `regedit`.
2.  Find `HKEY_LOCAL_MACHINE\Software\Google\DriveFS`
3.  Create DWORD `DisableLocalizedVirtualFolders`.
4.  Change the value to `1`.

Restart the Google Drive for desktop software. It now shows "My Drive". Job done!
