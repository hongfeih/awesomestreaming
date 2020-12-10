---
description: 流媒体中的DRM
---

# DRM-1

## 什么是DRM?

Digital rights management \(DRM\) 是一种对数字媒体版权保护的系统化方法，目的就是防止数字媒体的未授权分发和限制消费者播放内容的方式。 对于流媒体而言，DRM就是加密媒体内容，使得未授权用户在没有取得合法媒体密钥的情况下无法播放，或者限制授权用户在限定设备或时间范围内进行播放。

## Prepare contents

You need HLS to reach Apple devices, and DASH for most other devices.  DASH supports a range of third-party DRM solutions like Widevine and PlayReady \([CENC](./#cenc)\). Regarding HLS, generally Apple’s FairPlay \([Sample-AES](./#sample-aes)\) is the only one, but recently with the blooming of HLS CMAF, player can use DASH-alike DRM solutions as well.

| OS | Browser | DRM |
| :--- | :--- | :--- |
| Android 4.4+ |  | DASH/HLS CMAF + Widevine |
| iOS 9+ |  | HLS + FairPlay |
| Windows 10 | Chrome v35+ | DASH/HLS CMAF + Widevine |
|  | Firefox v47+ | DASH/HLS CMAF + Widevine |
|  | Opera v31+ | DASH/HLS CMAF + Widevine |
|  | MS Edge \(Chromium\) | DASH/HLS CMAF + Widevine |
|  | IE v11+ | DASH/HLS CMAF + PlayReady |
|  | MS Edge | DASH/HLS CMAF + PlayReady |
| Windows 8.1 | Chrome v35+ | DASH/HLS CMAF + Widevine |
|  | Firefox v47+ | DASH/HLS CMAF + Widevine |
|  | Opera v31+ | DASH/HLS CMAF + Widevine |
|  | MS Edge \(Chromium\) | DASH/HLS CMAF + Widevine |
|  | IE v11+ | DASH/HLS CMAF + PlayReady |
| Windows 7 | Chrome v35+ | DASH/HLS CMAF + Widevine |
|  | Firefox v47+ | DASH/HLS CMAF + Widevine |
|  | MS Edge \(Chromium\) | DASH/HLS CMAF + Widevine |
| Mac OS X 10.9+ | Safari 9+ | HLS + FairPlay |
|  | Chrome v35+ | DASH/HLS CMAF + Widevine |
|  | Firefox v47+ | DASH/HLS CMAF + Widevine |
|  | Opera v31+ | DASH/HLS CMAF + Widevine |
|  | MS Edge \(Chromium\) | DASH/HLS CMAF + Widevine |

As a trend, the DRM market prefer to use a single set of CBC-encrypted files that can support all DRMs via a specification called the Common Media Application Format \(CMAF\). That's a good story, however, there are a significant number of legacy DASH and HLS devices and players which don't support, so you have to provide several sets of files for HLS and DASH separately in a relatively long term.

Let's talk about following 2 most important topics.

### Encryption

#### CENC

ISO/IEC 23001-7 Common encryption in ISO base media file format files

#### Sample-AES

### Packaging

### HLS

#### DASH

## DRM Providers

### Widevine

### PlayReady

### FairPlay

## A DRM-capable player

### HTML5

#### Encrypted Media Extensions \(EME\)



## References

1. [How to Protect Your Content With DRM](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/How-to-Protect-Your-Content-With-DRM-132289.aspx)

