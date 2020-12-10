# 流媒体中的DRM

## 什么是DRM?

Digital rights management \(DRM\) 是一种对数字媒体版权保护的系统化方法，目的就是防止数字媒体的未授权分发和限制消费者播放内容的方式。 对于流媒体而言，DRM就是加密媒体内容，使得未授权用户在没有取得合法媒体密钥的情况下无法播放，或者限制授权用户在限定设备或时间范围内进行播放。

## Prepare contents

You need HLS to reach Apple devices in the browser, and you’ll need DASH for most other devices. Currently, this requires two sets of files, one for HLS and one for DASH, though this will change over the next few years \(more on this below\). Currently, the only DRM supported with HLS is Apple’s FairPlay. In contrast, DASH supports a range of third-party DRM solutions like Widevine and PlayReady.

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

Via a specification called the Common Media Application Format \(CMAF\), the DRM market is moving toward a single set of CBC-encrypted files that can support all three DRMs. In the short term, however, this approach wouldn’t work for a significant number of legacy DASH and HLS devices and players, which is why most producers either support two sets of files or use dynamic packaging to create the appropriate DASH or HLS content for each player.

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

