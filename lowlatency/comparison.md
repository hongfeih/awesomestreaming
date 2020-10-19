---
layout: default
title:  "Comparison of DASH-LL and LL-HLS"
categories: lowlatency
parent: Low Latency
nav_order: 1
---

# Comparison of DASH-LL and LL-HLS
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---



## Use of Chunked Transfer Encoding (CTE)

DASH-LL uses CTE:

- Pros:
  - One request per segment
  - Deliver the sequential data in correct order
  - Allow small chunk size (down to 1 frame)
- Cons:
  - Difficult to estimate throughput bandwidth for ABR 

While LL-HLS uses small independent chunk files:

- Pros:
  - 



![HLS blog 2_Comparison](https://www.theoplayer.com/hs-fs/hubfs/Blog/HLS%20blog%202_Comparison.png?width=660&name=HLS%20blog%202_Comparison.png)

## References

1. [Will Law - Three Roads to Jerusalem](https://www.youtube.com/watch?v=Col12gjnNlI)
2. [LL-HLS Series: How Does LL-HLS Work?](https://www.theoplayer.com/blog/how-does-ll-hls-work)

