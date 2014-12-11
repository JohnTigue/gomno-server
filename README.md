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
