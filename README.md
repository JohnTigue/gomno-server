gomno-server
============
Outbreak monitoring server for GOMNO. Functionality can be broken down into data-in, data-munge, and data-out:
- **data-in**: 
  - Data aggregators
    - Atom feed aggregation,
    - Web service reader and proxy
  - Data detection via #GOMNO hastag (in Twiiter or search indexes)
  - Email alert receiver and responder
- **data-munge**: 
  - Data is stored in HDFS
  - Main analysis is via Spark (scala)
  - Outbreak detection is via Spark calling into [The R-Package ’surveillance’](http://cran.r-project.org/web/packages/surveillance/vignettes/surveillance.pdf)
- **data-out**: 
  - HTML front including visualizations via **EbolaMapper** "Web Components"
  - Web service with Outbreak Time Series API data feed (node.js)
  - Atom feed with Outbreak Time Series XML namespaced into posts
    - Feed is also downcast to RSS with HTML payloads
  - Includes alarm notification via email
  
License is Apache 2.0. Contributors will need to sign a CLA as it is envisioned that new corporations, NGOs, and governments will deploy this codebase so the IPRs need to be clearly defined for the lawyers.

GOMNO is modern Internet technology for outbreak monitoring. It has some advanced, structured interfaces and highly scalable internals. Yet it is also explicitly designed to work with lower tech, semi-structured Internet data.
For example, on the high-end is an Atom feed with Outbreak Time Series Specification data namespaced in. On the low end, is the data detection via serch index. This can be simply a Web page containing the #GOMNO hashtag and an HTML table with Outbreak Time Series Spec'd data in it or a link to a CSV file which is conformant to the Outbreak Time Series Spec. GOMNO gets an alert from a search engines and then goes to fecth the data via HTTP.

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
	 - in@~9m: https://www.youtube.com/watch?v=MFSUAkDBSdQ&index=7&list=PL-x35fyliRwiDhtOvRSNgMdw05xFMzvhU

