---
layout: post
title: Jurisprudence Similarity Search Enginee
---

The generation of a jurisprudence similarity search engine involves three primary processes working together. Initially, it's essential to gather the relevant jurisprudence documents to populate the database. This database serves as the foundation from which the engine identifies the most similar documents based on their unique identifiers (IDs). This information is acquired through web scraping of the CENDOJ website. To accomplish this, the following sequential steps are undertaken:

1. **Data Collection (Web Scraping):** The first step involves collecting the necessary jurisprudence data to build the search engine's database. This data is sourced from the CENDOJ website using a web scraping tool. The tool, referred to as the "data_scraper," accesses the CENDOJ search platform and inputs specific parameters from the `arguments.json` file, such as dates and search queries. The search area for this explanation is set to Barcelona, although the user's ability to select locations might be added later. Given that each search returns a maximum of 200 documents, and to ensure comprehensive results, multiple searches are performed using the last jurisprudence date as a reference. Consequently, the total number of documents obtained is calculated as the "number of searches * 200."

2. **Scraping Jurisprudence Links:** The CENDOJ platform scraping process involves three critical stages: accessing the search site, extracting links to the embedded jurisprudence documents, and acquiring links to the final PDF versions. To obtain the final PDF link for a specific jurisprudence document, the process entails visiting the dynamic PDF link, extracting intermediate PDF links, and then accessing static PDF links that allow for text extraction. In essence, the scraper collects all available intermediate links to dynamic PDFs from the search platform, subsequently visiting each link to extract the static PDF link for comprehensive text scraping.

3. **Data Processing and Storage:** Once all static PDF links have been gathered, the data processing step begins, managed by the "data_preprocessor." This component's primary function is to extract textual content from the provided links, followed by the selection of pertinent information. The selected information is then stored in a localized SQLite database, ensuring accessibility for subsequent searches and analyses.

In summary, the creation of a jurisprudence similarity search engine involves collecting data from CENDOJ through web scraping, obtaining relevant PDF links, and then processing and storing the extracted information for future use.


<!-- <img src="images/scraping_explained.png" width="950" height="250" />*Visual explanation of all steps required to obtain the information that is stored into a local data base* -->