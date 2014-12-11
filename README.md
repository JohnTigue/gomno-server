gomno-server
============
Outbreak monitoring server for GOMNO. Functionality can be broken down into data-in, data-munge, and data-out:
- **data-in**
  - Data aggregators
    - Atom feed aggregation of Outbreak Time Series Spec's feeds
    - Web service (JSON) reader and proxier
  - Data detection via #GOMNO hastag (in Twiiter or search indexes)
  - Email alert receiver and responder
- **data-munge**
  - Data is stored as [Apache Parquet](parquet.incubator.apache.org/) in HDFS 
  - Main analysis is via Spark (scala)
  - Outbreak detection is via Spark calling into [The R-Package ’surveillance’](http://cran.r-project.org/web/packages/surveillance/vignettes/surveillance.pdf)
- **data-out**
  - HTML front including visualizations via **EbolaMapper** "Web Components"
  - Web service with Outbreak Time Series API data feed (node.js)
  - Atom feed with Outbreak Time Series XML namespaced into posts
    - Feed is also downcast to RSS with HTML payloads
  - Includes alarm notification via email
  
License is Apache 2.0. Contributors will need to sign a CLA as it is envisioned that new corporations, NGOs, and governments will deploy this codebase so the IPRs need to be clearly defined for the lawyers.

## Status
This project was started in mid December 2014, so these are early days. **This document is really a roadmap of what will be done. Docs are ahead of working code. This porject's GitHub issues tracker is the place to dig into the nitty gritty of where things are at. The following is a very high level status summary of what is currently working.
- **data-in**
  - Web service (JSON) readers for ebolainliberia.org, https://data.hdx.rwlabs.org/ebola, and similar.
- **data-munge**
  - Data is stored as [Apache Parquet](parquet.incubator.apache.org/) in HDFS 
- **data-out**
  - Web service JSON with Outbreak Time Series API data feed (node.js) 

Next steps:
- CSV injestion, specifically the [Caitlin Rivers' CSVs](https://github.com/JohnTigue/EbolaMapper/wiki/Other-Coders#caitlin-rivers)
- CSV generation. Export outbreak data in Parquet file to CSV comformant to the Outbreak Time Series Specification.
- **EbolaMapper** visualizations of the 2014 Ebola in West Africa Outbreak


## Overview
GOMNO is modern Internet technology for outbreak monitoring. It has some advanced, structured interfaces and highly scalable internals. Yet it is also explicitly designed to work with lower tech, semi-structured Internet data.
For example, on the high-end is an Atom feed with Outbreak Time Series Specification data namespaced in. On the low end, is the data detection via serch index. This can be simply a Web page containing the #GOMNO hashtag and an HTML table with Outbreak Time Series Spec'd data in it or a link to a CSV file which is conformant to the Outbreak Time Series Spec. GOMNO gets an alert from a search engines and then goes to fecth the data via HTTP.

The "N" is GOMNO is for "Network." GOMNO is architeched for a decentralized network. There should be no central authority so that in the case of dire global breakdown the network can continue to function. Certainly, certain high statused organizations will become the go-to places for information on outbreaks. For example, the UN's WHO could be a significant player. 

GOMNO is an acronym for Globlal Outbreak Monitoring Network Organization. The Organization is simply an open group of interested parties who congregate to define, enable, and support a Global Outbreak Monitoring Network. The Network is designed to be decentralized. Data is easily aggregated. So, an individual goverment could deploy a GOMNO server for collecting their data, they could publish the data openly on the Internet for aggregation. On the ohter hand, if they are concerned with keeping the information more privately controlled, they can deploy a GOMNO server and, say only share the data with WHO or their partner at Oxford, SEEG. 

## Why spark is used for GOMNO 
- GOMNO needs to be globally scalable. 
- It is a GOMNO requirement to run on all open source code (AWS as a deploy target is possible but MUST NOT be a requirement)
- The "volume" aspect of "big data" is really not significant, at this time.
- Yet put in terms of [IBM's 4 "V"s of big data](http://www.ibmbigdatahub.com/infographic/four-vs-big-data), this project has 3 out of 4 problems already. Variety and veracity are already issues. Volume and velocity should be expected, especially if dealing with email and twitter. 

Spark was chosen because:
- Most active big data open source project. In Apache.
- Good at statistics, OLAP, and data munging including loosely structured data (such as coming from multiple sources)
- Plays well with existing Java and R tools and the Hadoop ecosystem
- Spark 1.1 has nice JSON tools; "No need to define JSON schema, no ETL, easy to access complex structures"
  - see in@~9m: [Easy JSON Data Manipulation in Spark - Yin Huai (Databricks)](https://www.youtube.com/watch?v=MFSUAkDBSdQ&feature=player_detailpage&list=PL-x35fyliRwiDhtOvRSNgMdw05xFMzvhU#t=549)
- Spark can deal with streaming situations, such as Twitter streams of (hopefully) geotagged #GOMNO tagged messages, as well as [standard UN emergency hastags](http://www.unocha.org/node/117960) which during emergencies can ["often reaching up to thousands of postings and hundreds of photos per minute"](http://www.aljazeera.com/indepth/opinion/2014/11/how-tweets-algorithms-can-sav-20141130142519956906.html).
