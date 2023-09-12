---
layout: post
title: Jurisprudence Similarity Search Enginee
---

The generation of a jurisprudence similarity search engine involves three primary processes: data retrieval, similarity system fitting and search engine infraestructure and backend. This post aims to do an in-deph walk throug those processes. If you find any loose end, please add a comment and I will invest time in expanding a section adding the corresponding information.

## Data retrieval and processing

Initially, it's essential to gather the relevant jurisprudence documents to populate the database. This database serves as the foundation from which the engine identifies the most similar documents based on their unique identifiers (IDs). This information is acquired through web scraping of the CENDOJ website.

To obtain this documents, first it is required to scrape the links found in the searcher results. Given that each search returns a maximum of 200 documents divided in pages with a maximum of 20 documents. To ensure comprehensive results, multiple searches are performed using the last jurisprudence date as a reference. Consequently, the total number of documents obtained is calculated as the "number of searches * 200". Those links drive to a website where is located another link that points to the PDF of a jurisprudence document from which the textual information can be extracted.

Consecuently, the driver will have to access a total of "number of searches * 20 + 200 * number of searches" times the CENDOJ website. This is a synonym of getting blocked at the first try if not careful. Furthermore, scraping is a fail and retry process, It is very likely the scraper finds any problem and gets interrumped, we have taken measures not to lose all the information if the process suddenly stops and to automatically be able to resume scraping re-executing the main command.

### Scrape CENDOJ search results

To scrape the CENDOJ searcher, first of all, the specific parameters have to be inputed to the `arguments.json` file, such as dates and search queries. Once settings are defined, the preprocessor will open the CENDOJ website and input the parameter values, a total of `number_of_searches` will be performed, from each one, a maximum of 200 results will be displayed, organized in pages of 20. Every result will contain an _intermediate link_ and its date. This link is saved into a numpy array and the last date from a given search and its round are saved. This way, if the process stops, it can be resumed to the last date scraped and the same round. Furthermore, no duplicate links are saved, a set is used to gather urls in order to guarantee this.

### Scrape jurisprudence pdf links

Once the first process has finished, the second part triggers, which consists in accessing every _intermediate link_ and extracting the link to the jurisprudence document. This process one of the most intensives, as a high number of requests will be generated to the CENDOJ website and it is likely that problems arise. For this reason, a SQLite is created `jurisprudence.db` in which a table `jurisprudence_urls` will store the _intermediate links (base link)_ from which a _jurisprudence link (final link)_ has been retrieved. This way, all information is stored when obtained and if the process fails anytime, it can be resumed with 0 loss.

### Textual information extraction, organization and storage

Once all static PDF links have been gathered, the data processing step begins. This component's primary function is to extract textual content from the provided links, followed by the selection of pertinent information. The selected information is then stored in the SQLite database `jurisprudence.db`, in the table `jurisprudence_info`, ensuring accessibility for subsequent searches and analyses. The following information is stored:

    - id_cendoj: The CENDOJ id extracted from the document.

    - date: The date extracted from the document.

    - keyphrases: The content under the section "Cuestiones" in the document.

    - recurring_part: The content under the section "Parte recurrente/apelante" in the document.

    - appellant: The content under the section "Parte recurrida/apelada" in document

    - factual_background: The content under the section "Antecedentes de hecho" in the document.

    - factual_grounds: The content under the section "Fundamentos" in the document.

    - verdict_arguments: The content under the section "Fallo" in the document.

    - first_verdict: The first fallo extracted from the antecedentes section.

    - last_verdict: The fallo definitivo extracted from the document.

    - legal_costs: A flag (1 or 0) indicating if "Costas Procesales" were found in document.


<img src="/images/scraping_explained.png" width="900" height="220" />*Visual explanation of all steps required to obtain the information that is stored into a local data base*