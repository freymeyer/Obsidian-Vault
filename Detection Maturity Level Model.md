---
tags:
  - "#cybersecurity/blue-team/detection-engineering"
---

[Ryan Stillions](http://ryanstillions.blogspot.com/2014/04/the-dml-model_21.html) brought forward the Detection Maturity Level (DML) model in 2014 as a way for an organisation to assess its maturity levels concerning its ability to ingest and utilise cyber threat intelligence in detecting adversary actions. According to Ryan, there are two guiding principles for this model:

1. An organisation's maturity is not measured by its capabilities of obtaining valuable intelligence but by its ability to apply it to detection and response.
2. Without established detection functions, there is no opportunity to carry out response functions.

The DML model comprises nine dedicated maturity levels, numbered from 0 to 8, with the lowest value representing technical aspects of an attack and the highest level representing abstract and intelligence-based aspects of an attack. The individual levels can be described as follows:

- **DML-8 Goals:** The pinnacle of the model represents organisations that can detect an adversary's motive and goals. Unfortunately, it is near impossible to conduct detections solely based on goals, as in most cases, it is a guessing game based on behavioural findings from lower DMLs.
- **DML-7 Strategy**: Following closely after DML-8, this level is non-technical and represents the adversary's intentions and strategies to fulfil them. Organisations at this level would have a mature intelligence source that will ensure they have context about an adversary's plans, which will be helpful to responders.
- **DML-6 Tactics:** Organisations must be able to detect a tactic being used by an adversary without necessarily knowing which technique or tool they used. Tactics are detectable after observing patterns of events that aggregate over time and conditions.
- **DML-5 Techniques:** Techniques usually are specific to an individual or APT. Therefore, adversaries leave behind evidence of their attack habits and behaviours and organisations that can detect when a particular threat actor is within their environment are at an advantage.
- **DML-4 Procedures:** Organisations require to detect sequences of events from an adversary at this level. They will be very organised and follow a given pattern, such as the pre-exfiltration reconnaissance.
- **DML-3 Tools:** Detection of tools can fall into two phases: the `transfer phase` where the tool is downloaded via the network onto a host device and resides on a file system or in memory. And the second is detecting through the tool's `functionality and operation`. In some cases, this detection level would require organisations to perform reverse engineering against adversarial tools, making it difficult to cause havoc by understanding their tools' capabilities.
- **DML-2 Host & Network Artefacts:** Most organisational resources would be spent gathering IOCs and artefacts as threat intel at this level. Unfortunately, in most cases, indicators are observed after the fact. The threat actor would likely be causing havoc within the network when artefacts are picked up and investigated. This has been described as "chasing the vapour trail of an aircraft".
- **DML-1 Atomic Indicators:** This level comprises organisations utilising threat intel feeds in the form of lists of IP addresses and domains to detect threats.
- **DML-0 None:** At the bottom of the model, organisations that operate at this level have no detection processes established.

![[Pasted image 20251204162121.png]]

In the original publication of the DML model, Ryan described four critical use cases for the model, namely:

1. To provide a lexicon for more accessible communication of threat information.
2. To assess detection maturity against monitored threat actors.
3. To assess the maturity of security vendors and products in use.
4. To provide context to analysts