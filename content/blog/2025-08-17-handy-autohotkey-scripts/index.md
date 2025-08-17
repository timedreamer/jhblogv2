---
title: Handy AutoHotKey scripts
author: Ji Huang
date: '2025-08-17'
slug: []
categories:
  - dry lab
tags:
  - tools
  - Windows
meta_img: images/image.png
description: Description for the page
---

Here are three handy AutoHotKey version 2 scripts I use. 

If you want to start when turning on the computer, save them in the folder `C:\Users\td\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`.


## 1. Use `CapsLock` as `Esc`

`Caps2ESC.ahk`

```ahk
#Requires AutoHotkey v2.0

CapsLock::Send("{Escape}")

```

## 2. Use Left `Alt` + H,J,K,L to move the cursor

`AltArrowMovement.ahk`

```ahk
LAlt & j::Send("{Down}")
LAlt & l::Send("{Right}")
LAlt & h::Send("{Left}")
LAlt & k::Send("{Up}")
```

## 3. Use `CTRL+ATL+T` to type a time stamp

`timeStamp.ahk`

```ahk
; AutoHotkey v2 Script: Insert Formatted Timestamp
;
; This script inserts the current date and time in the format "YYYY-MM-DD h:mm am/pm"
; when the user presses the specified hotkey.
;
; Hotkey: Ctrl+Alt+T (^!t)
;   ^ represents Ctrl
;   ! represents Alt
;   t is the letter 't'
; You can change this to any combination you like, e.g., #t for Win+T

#Requires AutoHotkey v2.0+

; --- Hotkey Definition ---
^!t::
{
    ; Get the current date and time and format it.
    ; yyyy = 4-digit year
    ; MM   = 2-digit month (01-12)
    ; dd   = 2-digit day of the month (01-31)
    ; h    = 1 or 2-digit hour in 12-hour format (1-12)
    ; mm   = 2-digit minute (00-59)
    ; tt   = AM/PM designator in lowercase (am/pm)
    formattedTime := FormatTime(, "yyyy-MM-dd h:mmtt")

    ; Send the formatted time as keystrokes to the active window.
    Send(formattedTime)
}

```