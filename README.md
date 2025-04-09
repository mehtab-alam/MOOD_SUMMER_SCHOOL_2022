<p> <img src="https://mood-h2020.eu/wp-content/uploads/2020/10/logo_Mood_cmjn_black-1.jpg" width="15%";"/></p>

## **Practical : Event Classification**









**Mehtab Alam Syed**,
**Nejat Arinik**,
**Mathieu Roche**



**MOOD Summer School 2022**

---

# Before starting

If you want to just run all the code, we need to do it in **two** steps.
1. First, go to the menu and click this option: *Runtime > Run all*
  * It installs the necessary python packages and forces to restart the Google Colab runtime session. This restarting process is needed in order to take into account the installed packages. It takes approximately 5 mins.
2. Then, go to the section [*Importing Necessary Libraries*](#import_library) and just click the cell below (i.e. the cell starting with "*import pandas as pd*"). Then, go to the menu and click this option: *Runtime > Run after*
  * It executes all the cells starting from the current one
3. (optional) If you want to restart from scratch, then go to the menu and click this option: *Runtime > Disconnect and delete runtime*. Then, go to step 1.
4. The overall running process takes approximately 20-25 mins.

# Introduction

This notebok is used for the "Mining Media Data" session of the MOOD Summer School 2022.

**Context:** Avian Influenza (AI) is a highly contagious animal disease, which mainly infects wild and domestic bird species. Transmission between birds can be direct due to close contact between birds, or indirect through contaminated materials such as feed and water. The emergence and spread of AI has serious consequences for animal health and a substantial socio-economic impact for worldwide poultry producers. Due to its highly contagious nature, it is critical to monitor the evolution and spread of this disease.

There are mainly two types of surveillance systems established for this purpose: Indicator-Based Surveillance (IBS) and Event-Based Surveillance (EBS). IBS usually relies on official notification procedures submitted by a country, whereas EBS extracts epidemiological events mostly from  unofficial sources, such as online news articles, through various Natural Language Processing tasks. Recently, several EBS platforms have shown their effectiveness by detecting the first signals of emerging infectious disease outbreaks in a timely manner and providing alerts within previously unaffected areas.

The standard pipeline of such an EBS platform consists of four tasks:
1. Data collection: This step consists in collecting daily news articles from a news aggregator (e.g. Google News) through disease-based RSS (Really Simple Syndication) feeds by using disease-specific terms (e.g. avian flu OR avian influenza OR bird flu).
2. Data processing: This step consists in retrieving the content of news articles by removing irrelevant elements (pictures, ads, hyperlinks, etc.).
3. Data classification: It is possible that a retrieved news article containing disease-specific keywords is not related to epidemiological disease information. For this reason, this step consists in identifying epidemiological disease-related articles from the whole dataset. In other words, it amounts to classify the news articles into two categories: relevant vs. irrelevant articles.
4. Information extraction: This step consists in extracting epidemiological event-related information from relevant articles.


**Objective:** Our ultimate goal is to build a very simplified EBS system, that we call it *Simple Avian Flu Tracker (SAFT)*. In this notebook, we will focus on only the data classification task of SAFT in order to distinguish the relevant AI articles from unrelevant ones. This amounts to perform a binary text classification task.

We have already downloaded a set of news articles with Avian Influenza-related keywords from Google News and extracted their processed contents. It is composed of 204 articles, with information about the article itself (publication date, title, content, url, etc.). You can download the dataset from the link : [LINK](https://drive.google.com/file/d/1_ls-oDHdCMU2DoPtqX4az8dqZsbO-qNj/view?usp=sharing).

Furthermore, we manually annotated these articles into two categories: Avian Influenza-related disease event information vs. irrelevant. The first category can be of the following types : 1) a new oubtreak information, 2) alert and preparedness, 3) consequences.



**Examples of relevant articles:**

> *Denmark will cull nearly 9,000 poultry after a strain of the bird flu  virus was found on the farm in the village of Lovel on the borders of the Viborg Municipality in the country's Jutland Peninsula, according to local authorities on Saturday.The H5N8 type of the avian influenza virus was detected among the poultry in a farm during controls by the State Serum Institute on the first day of the year, according to a statement by the Food and Control Administration.Around 9,000 poultry on the farm will be culled to prevent the spread of the virus, said the authority, asking for measures to be taken against wild birds in poultry farms.*


> *The Western Cape government says the effect of bird flu on jobs is concerning. More than 24,000 birds have been culled on The Duck Farm in Joostenbergvlakte following an outbreak of avian flu. Last week, at least 30,000 chickens were killed due to the deadly flu strain hitting a commercial poultry farm in the Paardeberg region. Three ostrich farms in the Heidelberg area were also affected, and although none of the large birds have died, they remain under quarantine. Nearly a hundred workers on The Duck Farm have lost their jobs due to the loss of thousands of ducks.*



**Examples of irrelevant articles:**

> *After raging bird flu in iława, new town and departmental district, the trouble continued. An outbreak of ASF was detected on the farm in rybno municipality. Accordingly, by decision of the Voivode of Warmia and Mazury Ostaszewo and Zwiniarz were included in the area at risk of swine disease. African swine fever (ASF) is a rapidly spreading infectious viral disease to which domestic pigs and feral pigs are susceptible.*

> *The avian influenza has not yet completely expired (in June studies showed the occurrence of the 337th outbreak of HPAI), and Polish farms are again beginning to touch African swine fever. The Chief Veterinary Inspectorate reported a further three outbreaks of ASF in 2021. Outbreaks of African swine fever are arriving.*

> *Indonesia already has experiences and capabilities in handling outbreaks since it was the country worst-affected by the avian influenza ( H5N1 ) outbreak that killed 168 people in the country and 286 in the rest of the world between 2003 and 2017, according to the WHO.
While none were fatal, the country also handled cases of Severe Acute Respiratory Syndrome ( SARS ) and the Middle Eastern Respiratory Syndrome ( MERS ) -- other ailments caused by different strains of coronavirus. During the nine month outbreak from 2002 to 2003, SARS, first reported in Guangzhou, China, killed more than 775 people, mostly in China and Hong Kong. Initially discovered in Jordan in 2012, MERS caused 858 deaths up until last year, mostly in Saudi Arabia. Until the WHO issues a new protocol to handle the current outbreak, it would be better for the country to exercise the same level of preparedness implemented during the bird flu emergency.*
