---
layout:     post
title:      "Android 多种屏幕分辨率支持"
date:       2013-11-08 10:12:07
categories: [Android]
tags:       [Android, Multiple screens]
---

Android下支持多种屏幕分辨率支持。
<!--more-->

### 1. 常见的屏幕分辨率

|                         | Low density (120), ldpi                   | Medium density (160), mdpi                        | High density (240), hdpi                          | Extra high density (320), xhdpi |
| ----------------------- | ----------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------------- |
| Small screen            | *QVGA (240x320)*                          |                                                   | 480x640                                           |                                 |
| Normal screen           | *WQVGA400 (240x400)* *WQVGA432 (240x432)* | *HVGA (320x480)*                                  | *WVGA800 (480x800)* *WVGA854 (480x854)*  600x1024 | 640x960                         |
| Large screen            | *WVGA800 (480x800)* *WVGA854 (480x854)*   | *WVGA800 (480x800)*  *WVGA854 (480x854)* 600x1024 |                                                   |                                 |
| Extra Large screen      | 1024x600                                  | *WXGA (1280x800)* 1024x768 1280x768               | 1536x1152 1920x1152 1920x1200                     | 2048x1536 2560x1536 2560x1600   |

### 2. 屏幕分辩率和资源文件

| Screen characteristic | Qualifier | Description |
| --------------------- | --------- | ----------- |
| Size         | small | Resources for _small_ size screens. |
|| normal      | Resources for _normal_ size screens. (This is the baseline size.) |
|| large       | Resources for _large_ size screens. |
|| xlarge      | Resources for _extra large_ size screens. |
| Density      | ldpi | Resources for low-density (_ldpi_) screens (~120dpi). |
|| mdpi        | Resources for medium-density (_mdpi_) screens (~160dpi). (This is the baseline density.) |
|| hdpi        | Resources for high-density (_hdpi_) screens (~240dpi). |
|| xhdpi       | Resources for extra high-density (_xhdpi_) screens (~320dpi). |
|| nodpi       | Resources for all densities. These are density-independent resources. The system does not scale resources tagged with this qualifier, regardless of the current screen's density. |
|| tvdpi       | Resources for screens somewhere between mdpi and hdpi; approximately 213dpi. This is not considered a "primary" density group. It is mostly intended for televisions and most apps shouldn't need it—providing mdpi and hdpi resources is sufficient for most apps and the system will scale them as appropriate. If you find it necessary to provide tvdpi resources, you should size them at a factor of 1.33*mdpi. For example, a 100px x 100px image for mdpi screens should be 133px x 133px for tvdpi. |
| Orientation  | land | Resources for screens in the landscape orientation (wide aspect ratio). |
|| port        | Resources for screens in the portrait orientation (tall aspect ratio). |
| Aspect ratio | long | Resources for screens that have a significantly taller or wider aspect ratio (when in portrait or landscape orientation, respectively) than the baseline screen configuration. |
|| notlong     | Resources for use screens that have an aspect ratio that is similar to the baseline screen configuration.|
