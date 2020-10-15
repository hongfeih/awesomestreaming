---
layout: default
title:  "Low Latency HLS from player perspective"
categories: lowlatency
parent: Low Latency
nav_order: 2
---

# Low Latency HLS from player perspective
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Declare supported features

The EXT-X-SERVER-CONTROL tag allows the Server to indicate support for Delivery Directives:

| Item                | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| CAN-SKIP-UNTIL      | Indicates that the Server can produce Playlist Delta Updates in response to the _HLS_skip Delivery Directive.  Its value is the Skip Boundary, a decimal-floating-point number of seconds. The Skip Boundary MUST be at least six times the Target Duration. |
| CAN-SKIP-DATERANGES | A value of YES indicates that the Server can produce Playlist Delta Updates that skip older EXT-X-DATERANGE tags in addition to Media Segments. |
| HOLD-BACK           | The value is a decimal-floating-point number of seconds that indicates the server-recommended minimum distance from the end of the Playlist at which clients should begin to play or to which they should seek, unless PART-HOLD-BACK applies.  Its value MUST be at least three times the Target Duration.<br/>This attribute is OPTIONAL.  Its absence implies a value of three times the Target Duration.  It MAY appear in any Media Playlist. |
| PART-HOLD-BACK      | The value is a decimal-floating-point number of seconds that indicates the server-recommended minimum distance from the end of  the Playlist at which clients should begin to play or to which they should seek when playing in Low-Latency Mode.  Its value MUST be at least twice the Part Target Duration.  Its value SHOULD be at least three times the Part Target Duration.  If different Renditions have different Part Target Durations then PART-HOLD-BACK SHOULD be at least three times the maximum Part Target Duration. |
| CAN-BLOCK-RELOAD    | The value is an enumerated-string whose value is YES if the server supports Blocking Playlist Reload. |



## Live Edge Calculation

Similar with Latency@target of LL-DASH, **PART-HOLD-BACK** is the service provider’s preferred presentation latency, which can be used to calculate live edge. 

>  Player SHOULD NOT choose a segment closer to the end of the Playlist than described by the HOLD-BACK and PART-HOLD-BACK attributes.[1]

```
Find the part which:
- Calculate duration from last part reversely
- Find the start part which duration larger than PART-HOLD-BACK value and has INDEPENDENT=YES (Can be decoded independently, I-Frame)
```

For example:

```
#EXTM3U
# This Playlist is a response to: GET https://example.com/2M/waitForMSN.php?_HLS_msn=273&_HLS_part=2
#EXT-X-TARGETDURATION:4
#EXT-X-VERSION:6
#EXT-X-SERVER-CONTROL:CAN-BLOCK-RELOAD=YES,PART-HOLD-BACK=1.0,CAN-SKIP-UNTIL=12.0
#EXT-X-PART-INF:PART-TARGET=0.33334
#EXT-X-MEDIA-SEQUENCE:266
#EXT-X-PROGRAM-DATE-TIME:2019-02-14T02:13:36.106Z
#EXT-X-MAP:URI="init.mp4"
#EXTINF:4.00008,
fileSequence266.mp4
#EXTINF:4.00008,
fileSequence267.mp4
#EXTINF:4.00008,
fileSequence268.mp4
#EXTINF:4.00008,
fileSequence269.mp4
#EXTINF:4.00008,
fileSequence270.mp4
#EXT-X-PART:DURATION=0.33334,URI="filePart271.0.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.1.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.2.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.3.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.4.mp4",INDEPENDENT=YES
#EXT-X-PART:DURATION=0.33334,URI="filePart271.5.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.6.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.7.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.8.mp4",INDEPENDENT=YES
#EXT-X-PART:DURATION=0.33334,URI="filePart271.9.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.10.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart271.11.mp4"
#EXTINF:4.00008,
fileSequence271.mp4
#EXT-X-PROGRAM-DATE-TIME:2019-02-14T02:14:00.106Z
#EXT-X-PART:DURATION=0.33334,URI="filePart272.a.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.b.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.c.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.d.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.e.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.f.mp4",INDEPENDENT=YES
#EXT-X-PART:DURATION=0.33334,URI="filePart272.g.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.h.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.i.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.j.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.k.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart272.l.mp4"
#EXTINF:4.00008,
fileSequence272.mp4
#EXT-X-PART:DURATION=0.33334,URI="filePart273.0.mp4",INDEPENDENT=YES
#EXT-X-PART:DURATION=0.33334,URI="filePart273.1.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart273.2.mp4"
#EXT-X-PRELOAD-HINT:TYPE=PART,URI="filePart273.3.mp4"

#EXT-X-RENDITION-REPORT:URI="../1M/waitForMSN.php",LAST-MSN=273,LAST-PART=2
#EXT-X-RENDITION-REPORT:URI="../4M/waitForMSN.php",LAST-MSN=273,LAST-PART=1
```

- PART-HOLD-BACK=1.0, means desired latency target is 1.0s

- Calculate duration to 1.0s from last part ("**filePart273.2.mp4**") reversely, locate start part "**filePart273.0.mp4**"

  ```
  0.33334 + 0.33334 + 0.33334 > 1.0
  ```

-  start with part "**filePart273.0.mp4**" as it has property "INDEPENDENT=YES"

  - Or need continue to find the one with "INDEPENDENT=YES"



## Preload hints and blocking of Media downloads

Eliminating unnecessary round trips is critical when delivering low-latency streams at global scale. Servers use a new tag, EXT-X-PRELOAD-HINT, to inform clients of upcoming Partial Segments and Media Initialization Sections. A client can issue a GET request for a hinted resource in advance; the server responds to the request as soon as the media becomes available.

```
#EXT-X-PRELOAD-HINT:<attribute-list>
```

For example

```
#EXT-X-PART:DURATION=0.33334,URI="filePart273.0.mp4",INDEPENDENT=YES
#EXT-X-PART:DURATION=0.33334,URI="filePart273.1.mp4"
#EXT-X-PART:DURATION=0.33334,URI="filePart273.2.mp4"
#EXT-X-PRELOAD-HINT:TYPE=PART,URI="filePart273.3.mp4"
```

After part "filePart273.2.mp4" is downloaded, player don't need to wait for playlist reload, instead, can try to get "filePart273.3.mp4" directly.



## Blocking of Playlist reload



## Generation of Partial Segments



## Playlist Delta Updates



## Rendition Reports



## References

1. [HLS specification](https://tools.ietf.org/html/draft-pantos-hls-rfc8216bis-07)
2. [Enabling Low-Latency HLS](https://developer.apple.com/documentation/http_live_streaming/enabling_low-latency_hls)
3. [Update: What is Low-Latency HLS and How Does It Relate to CMAF](https://www.wowza.com/blog/apple-low-latency-hls)
4. [Ultra-Low-Latency Streaming Using Chunked-Encoded and ChunkedTransferred CMAF](https://www.akamai.com/us/en/multimedia/documents/white-paper/low-latency-streaming-cmaf-whitepaper.pdf)
5. [Video Tech Deep-Dive: Live Low Latency Streaming Part 3 – Low-Latency HLS](https://bitmovin.com/live-low-latency-hls/)