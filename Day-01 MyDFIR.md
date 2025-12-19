---
dg-publish: true
tags:
  - "#project/log/daily"
---
I’m excited to kick off Day 1 of the [hashtag#30DaySOCChallenge](https://www.linkedin.com/search/results/all/?keywords=%2330daysocchallenge&origin=HASH_TAG_FROM_FEED)!  
  
To start, I’ve designed the logical diagram for my home lab, focusing on a cloud-based architecture to simulate a real-world enterprise environment.  
  
The Tech Stack:  
🔹 SIEM: I’m deploying the ELK Stack (Elasticsearch, Logstash, Kibana). Elasticsearch will handle analytics, Logstash will ingest unstructured telemetry, and Kibana will serve as my visualization dashboard for monitoring logs.  
🔹 Ticketing: Integrated osTicket server to handle alerts and case management, simulating a true SOC workflow.  
🔹 Endpoints: Windows and Ubuntu servers acting as my "victim" machines, configured with RDP and SSH to generate traffic.  
🔹 Fleet Server: To manage agents and telemetry flow.  
  
I’ve opted for a Cloud-based VPC (Virtual Private Cloud) rather than local virtualization. This helps bypass hardware constraints while giving me hands-on experience with enterprise cloud standards. I'm applying networking concepts from my university coursework to manage the subnets ([10.10.10.0/24](http://10.10.10.0/24)).  
  
And, to test the Blue Team defenses, I’ll be operating a Kali Linux attack box and a Mythic C2 server to simulate Red Team activities.  
  
Please follow along with my DFIR Journey 👏  
  
[hashtag#CyberSecurity](https://www.linkedin.com/search/results/all/?keywords=%23cybersecurity&origin=HASH_TAG_FROM_FEED) [hashtag#SOCAnalyst](https://www.linkedin.com/search/results/all/?keywords=%23socanalyst&origin=HASH_TAG_FROM_FEED) [hashtag#BlueTeam](https://www.linkedin.com/search/results/all/?keywords=%23blueteam&origin=HASH_TAG_FROM_FEED) [hashtag#Homelab](https://www.linkedin.com/search/results/all/?keywords=%23homelab&origin=HASH_TAG_FROM_FEED) [hashtag#ELKStack](https://www.linkedin.com/search/results/all/?keywords=%23elkstack&origin=HASH_TAG_FROM_FEED) [hashtag#CloudSecurity](https://www.linkedin.com/search/results/all/?keywords=%23cloudsecurity&origin=HASH_TAG_FROM_FEED) [hashtag#Infosec](https://www.linkedin.com/search/results/all/?keywords=%23infosec&origin=HASH_TAG_FROM_FEED) [hashtag#Student](https://www.linkedin.com/search/results/all/?keywords=%23student&origin=HASH_TAG_FROM_FEED)