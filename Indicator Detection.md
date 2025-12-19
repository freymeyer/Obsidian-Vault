---
tags:
  - "#cybersecurity/blue-team/detection-engineering"
---

Indicators are pieces of information that identify a state and context of an element or entity. 

There are both `good` indicators used to identify legitimate activities or resources, such as those used in whitelists, and `bad` indicators used for suspicious or malicious resources, such as in blacklists or malware IPs.

[[Indicators of Compromise]] are commonly referenced and derived from investigations against malicious events. By observing threat activities and investigations, analysts can use identified indicators to craft detections and adapt them based on an adversary’s rate of change.

Some of the benefits and challenges of this detection method include the following:
- Fastest detection to create and deploy, but depends on the adversary's rate of change.
- Indicators raise specific threat contexts, but retroactive in nature and needs to observe the indicator first.
- Useful for enriching data sources and detections, but limited to some indicators that can be processed at a time.
- Practical for scoping environments post investigation of indicators, however unknown indicator expiry or change timelines can lead to false detections.