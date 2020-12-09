# 关于FairPlay Streaming

使用Apple FairPlay Streaming \(FPS\)可以将密钥安全传递给Apple移动设备，Apple TV以及OS X上的Safari，用于加密视频内容的播放。此处加密内容采用 HTTP Live Streaming \(HLS\)进行传输。

FPS保护用于解密视音频媒体的密钥的传输过程。Apple的设备或者电脑可以从内容提供商的密钥服务器安全的得到所需的密钥。操作系统（Mac OS/iOS）可以使用得到的密钥对媒体进行解密。

FPS的密钥传输提供了如下特性：

* AES-128 内容密钥由密钥服务器产生
* 仅有密钥服务器和Apple设备知道密钥
* 播放结束后，iOS设备，Apple TV或OS X上Safari中的密钥将从内存中永久删除
* 密钥服务器可以设定密钥的有效期
* 支持MPEG-2文件格式

FPS在发送密钥时会带着到期信息，设备可以据此随时停止视频播放。在Apple设备上使用FPS可以保证密钥在传输以及用于媒体解密时的安全。

## 概述 <a id="&#x6982;&#x8FF0;"></a>

Apple认证的FPS开发者可以在密钥服务器和播放应用中实现FPS以便在两端识别和支持FPS消息。当Apple设备播放一个在播放列表中包含FPS专用标识的媒体流时，OS会向app发起请求以获得解密的密钥：app调用一个API以启动FPS，OS就会为那个媒体的密钥准备加密的请求。当app发送这个请求到服务器时，服务器中的FPS代码将所需的密钥打包成加密的消息并返回给app；然后app请求OS解包这个消息用于这个媒体流的解密，这样，iOS设备、Apple TV和OS X上的Safari就可以播放加密的媒体了。

整个实现过程包括以下三个开发任务：

* 在密钥服务器软件中实现一个密钥安全模块（KSM）。该模块用于在FPS过程中和iOS设备、Apple TV和OS X上的Safari交换消息；
* 在Apple设备播放app中增加代码支持FPS。app可以和服务器进行交互得到用于加密电影等内容的密钥。图1-1为一个包括支持FPS播放的app的FPS系统的图示。
* 在媒体内容服务器中增加格式化和加密的软件。该软件可以根据Apple HLS标准准备加密的内容。

![&#x56FE;1-1 FPS &#x4EA4;&#x4E92;](../../.gitbook/assets/image%20%289%29.png)

## 密钥安全模块（KSM） 如何封装密钥用于传输

密钥安全模块（KSM）实现FPS的算法用于解释来自iOS设备、Apple TV和OS X上的Safari的加密的密钥请求消息，然后创建一个包含特定媒体密钥的加密的应答。密钥服务器中必须实现这些算法。

{% hint style="info" %}
相关章节：[KSM的开发](ksm-dev.md)
{% endhint %}

## 支持FPS的播放app如何向KSM申请密钥

支持FPS的播放app使用一个OS的编程接口去获取密钥的加密请求，发送给KSM，接收到用于解密媒体资源 的密钥。app同时需要实现支持用户交互的代码。

{% hint style="info" %}
相关章节：[开发支持FPS的应用](fps-app.md)
{% endhint %}

## FPS 如何安全地传输内容密钥

FPS用一个消息传递会话将内容密钥传输给播放器app。一个会话通常会这样进行：

1. app请求OS播放特定URL的内容；
2. OS访问播放内容并检查其播放列表（playlist）；
3. 播放列表中的一个属性表明该内容是用FPS的内容密钥加密的；
4. OS通知app该内容是用FPS加密的；
5. app请求OS准备一个用来请求密钥的FPS消息；
6. OS将一个加密的SPC消息返回给app；
7. app将这个SPC发送给包含KSM的密钥服务器；
8. KSM将SPC解密并从密钥服务器中获取所请求的内容密钥；
9. KSM将内容密钥打包在一个加密的CKC消息中并发送会app；
10. app将收到的CKC传递给OS中集成的FPS软件中用于媒体内容的解密。

这些FPS相关模块间的信息传递如图1-2所示。

![&#x56FE;1-2 FPS&#x4FE1;&#x606F;&#x6D41;](../../.gitbook/assets/image%20%287%29.png)

接收到CKC之后，OS中的FPS软件会提取出内容密钥并将其提供给OS，用于解密和播放媒体内容。

流媒体的播放列表（playlist）包含一个密钥服务器支持的FPS版本列表。OS识别并在其提供的加密密钥请求中列出其支持FPS版本，供密钥服务器挑选使用。

在整个过程中，app和服务器能够通过app开发者选择的任意的传输链路进行通信。媒体内容遵守HLS协议的要求，使用H.264音视频格式。

## 媒体服务器提供内容流

媒体服务器给iOS设备、Apple TV和OS X上的Safari提供格式化并加密的内容流。只有在Apple设备中，并使用通过FPS获取的密钥才可以解密和播放该内容。

{% hint style="info" %}
相关章节：[格式化和加密内容流](enc.md)
{% endhint %}

## FairPlay Streaming SDK 里有什么

Apple提供的FairPlay Streaming SDK有两个部分组成：

第一部分是FairPlay Streaming SDK，包含有一个密钥安全模块（KSM）的参考实现、一个客户端示例、标准以及一个测试集。测试集可以用来开发和测试KSM。

第二部分是FPS部署包，包含D函数、标准以及关于如何生成FPS证书，私钥和应用密钥（ASk）的指南。

这两个部分都是必须的，用于执行服务器KSM和客户端之间的往返事务交互。FPS部署包用于完成KSM的量产部署，必须由内容所有者申请。私钥和应用密钥应该在测试KSM时产生。

{% hint style="info" %}
相关章节：[FPS SDK及相关工具的使用](fps-tool.md)
{% endhint %}

## 参考资料

以下文档提供了该编程手册所需的标准和指南：

* FPS所采用的Apple流媒体技术的简要指南：[HTTP Live Streaming Overview](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/Introduction/Introduction.html)
* 关于FPS的媒体格式的信息：[MPEG-2 Stream Encryption Format for HTTP Live Streaming](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/Introduction/Introduction.html)
* IETF HLS标准的草案： [HTTP Live Streaming Protocol](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/Introduction/Introduction.html)
* 关于KSM的实现，可以参考D函数算法
* FPS相关的工业标准：
  * [Information technology—generic coding of moving pictures and associated audio information: Systems](http://www.itu.int/rec/T-REC-H.222.0)：ITU-T 建议的H.222.0标准，也发布为[ISO/IEC International Standard 13818-1:2013](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=62074). 
  * [Advanced video coding for generic audio visual services](http://www.itu.int/rec/T-REC-H.264)：ITU-T建议的H.264标准，页发布为[ISO/IEC International Standard 14496-10:2014](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=66069).
  * [Information technology—Coding of audio-visual objects—Part 3: Audio](http://www.iso.org/iso/catalogue_detail.htm?csnumber=42739)：[ISO/IEC International Standard 14496-3:2009](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=53943). 
  * [Digital Audio Compression Standard \(AC-3\)](http://atsc.org/standard/a522012-digital-audio-compression-ac-3-e-ac-3-standard-12172012/) 是Advanced Television Systems Committee \(ATSC\)  A/52:2012标准.

