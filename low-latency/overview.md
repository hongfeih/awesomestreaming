# Overview

## Where does live latency come from?

* Buffering ahead for playback stability at the player-level
  * Protocol introduced
  * Player maintained fro smooth playback

![](../.gitbook/assets/image%20%2810%29.png)

* Segments are produced, transferred and consumed in their entirety

![](../.gitbook/assets/image%20%2811%29.png)

## Low latency Solution

* Add low latency support into spec.
  * [DASH Low Latency from player perspective](dashll.md)
  * [Low Latency HLS from player perspective](llhls.md)
* Introduce CMAF \(Chunked encoding and transfer\)

