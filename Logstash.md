---
tags:
  - "#cybersecurity/blue-team/logging-siem"
---

Logstash is an open-source data ingestion tool that allows you to collect data from various sources, transform it, and send it to your desired destination. 

With prebuilt filters and support for over 200 plugins, Logstash allows users to easily ingest data regardless of the data source or type. 

Logstash is a lightweight, open-source, server-side data processing pipeline that allows you to collect data from various sources, transform it on the fly, and send it to your desired destination. It is most often used as a data pipeline for [[Elasticsearch]], an open-source analytics and search engine. Because of its tight integration with [[Elasticsearch]], powerful log processing capabilities, and over 200 prebuilt open-source plugins that can help you easily index your data, Logstash is a popular choice for loading data into [[Elasticsearch]].

## Easily load unstructured data
Logstash allows you to easily ingest unstructured data from various data sources including system logs, website logs, and application server logs. 
## Prebuilt filters
Logstash offers prebuilt filters, so you can readily transform common data types, index them in Elasticsearch, and start querying without having to build custom data transformation pipelines.
## Flexible plugin architecture
With over 200 plugins already available on GitHub, it is likely that someone has already built the plugin that you need to customize your data pipeline. But if one is not available that suits your requirements, you can easily create one yourself.