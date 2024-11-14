---
layout: post
title: "Analysis on FDA Drug Ozempic"
date: 2024-11-12T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Drugs", "EDA"]
description: This blog post will disucss my curated dataset from openFDA and webscrapped reviews on Ozempic from drugs.com.
thumbnail: "/assets/images/gen/blog/blog-22-thumbnail.webp"
image: "/assets/images/gen/blog/blog-22.webp"
comments: false

---

## openFDA and Ozempic

There has been a lot of talk about drug problems. And no, I'm not referring to the ones dicussed in intense immigration arguements. I'm talking about the drugs you get from the white angel of death, doctors! Or at least that's the opinion of some who have followed onto a recent popularity trend of using Ozempic. 

Ozempic is used for managing Type 2 diabetes but that is not what is captivating the attention of millions of Americans. It's Ozempics secondary, off-label effect, the drugs ability to help individuals lose weight. And lose weight you do, at an insane amount. Reports from users taking Ozempic have sworn the drug has caused them to lose to as much as 30 pounds in a month. In comparison, exercise and a clean diet, on average, nets you a loss of about 3-5 pounds per month.

It's litle wonder why so many have flooded clinics, hopeing to pull favors from doctors to get a prescription for the drug. And when not possible, many purchase for off-brand or out-of-country drug alternatives, hoping to regain their past body types again. I would know, as I have family members who have switched to Ozempic or similar drugs for managing their diabetes and also hope to cut a few pounds on the side. And believe me readers, the drug is not so miraculous when you analyze the adverse effects it causes users. Of course, what you hear on the news, ads, social media differs from optained 

It is because of this curiosity that I have created 2 seperate but important datasets for those who wish to explore the adverse effects of Ozempic. I will explain my reasoning in keeping these datasets seperate further into this blog.

My dataset project explores clinical and real-world data related to Ozempic's side effects and patient-reported experiences. By comparing clinical adverse event reports with user reviews, my datasets project aims to provide a clearer picture of the medication's impact on diverse patient demographics and highlight discrepancies or consistencies between clinical trials and everyday usage.

## Motivating Question

My dataset project seeks to answer the following: What are the most common and severe side effects associated with Ozempic, and how do they differ between clinical trial data and real-world patient experiences?

I will identify patterns in adverse reactions, assess user sentiment, and determine if specific side effects are underreported in clinical trials or emerge uniquely in real-world scenarios.

### Ethical Considerations and Good Scraping Practices

Data ethics and responsible data gathering were prioritized in this project. Here’s how I ensured ethical compliance:

- Data Source Verification: The data comes from publicly accessible resources—openFDA and Drugs.com, both of which were selected due to their allowance for data usage in research and analytical purposes. The datasets were curated to ensure its access aligns with both platform's guidelines.
- Personal Identifiability: The datasources were screened to ensure excluding any personally identifiable information (PII), protecting patient privacy.
- Respectful API Use: Data was collected using openFDA’s official API, which explicitly permits data extraction within reasonable limits. Rate limiting and data retrieval intervals were implemented to respect server load and prevent excessive requests.

### Steps for Data Collection

You can follow these steps to replicate a similar data gathering project:

1. Define your Data's Needs: Identify relevant fields, such as adverse reactions, patient demographics, and reported severity, to answer your motivating question.
2. Access openFDA Data: Register for an API key (if applicable) from openFDA and use it to request data on Ozempic’s adverse events.
3. Scrape Drugs.com User Reviews: Use a web scraping tool (I used Python's BeautifulSoup package) to extract user reviews, paying attention to rate limits and ethical scraping guidelines.
4. Store and Clean Data: Store data in CSV or JSON format, then clean it by handling missing values, categorizing reactions, and setting up sentiment analysis for user reviews.


### Key Metrics and Counts

Total Sample Size: Over 500 individual records of adverse events related to Ozempic from openFDA.

Counts of Categorical Variables:

- Adverse Reaction Type (reaction_meddra): Provides counts for each adverse reaction type, such as nausea, fatigue, and injection site pain, etc.
- Seriousness (serious): Categorizes cases as serious (1) or non-serious(0), with counts showing the proportion of severe cases.
- Patient Sex (patient_sex): Number of cases by patient sex by Male (1), Female (2), or Unspecified (3) .
- Drug Administration Route (drug_admin_route): Breakdown of the methods of administration, such as subcutaneous injection or other routes.

Numerical Summaries of Key Numeric Variables:

- Patient Age (patient_age): Summary statistics (ex: mean, median, minimum, maximum) to understand the age range of affected patients.
- Received Date (receivedate): The date when the adverse event was reported and received by the FDA. Useful for providing a timeline on adverse event reports and identifying trends/spikes over time (e.g., seasonal/social media patterns, post-approval phases).



### Conclusive Dataset Overview
This project uses two distinct but complementary datasets to examine Ozempic’s impact from both clinical and real-world perspectives. While these datasets are not merged, they offer unique insights when analyzed in parallel, allowing for a holistic view of adverse events and patient experiences.

1. Clinical Adverse Event Dataset (openFDA)

    The clinical dataset from openFDA provides detailed reports of adverse events based on official clinical and post-market surveillance data. This dataset covers:

    - Demographics: Information on patient age, sex, and country of origin, allowing for demographic analysis of clinical adverse events.
    - Side Effects: Categorized by type and severity, capturing common reactions (e.g., nausea, fatigue) and serious events, including those resulting in death.
    - Administration Methods: Specifies the route of administration, such as subcutaneous injection, offering insights into whether certain methods correlate with specific adverse effects.

    This dataset provides a structured, medically reviewed view of Ozempic’s side effects and severity, suitable for identifying clinically significant trends in adverse reactions.

2. User Sentiment Dataset (Drugs.com)
    
    The user sentiment dataset includes self-reported experiences from Ozempic users, gathered from Drugs.com reviews. Although less structured than the clinical dataset, this data adds depth by capturing patient-reported side effects, satisfaction, and sentiment in everyday use. This dataset covers:

    - Side Effects: Mentions of side effects by users, including commonly reported symptoms like nausea, fatigue, and weight changes, allowing for sentiment-based analysis of adverse reactions.
    - Sentiment Analysis: Patient reviews are categorized by sentiment polarity (positive, neutral, negative), providing a broader view of how users feel about the medication’s effects.
    - Real-World Experiences: Unlike the clinical data, this dataset includes nuanced, subjective reports, which may reveal side effects or concerns that are less prevalent in formal clinical reporting.

3. Complementary Insights

    Together, these datasets allow for comparative analysis, showing both the frequency and severity of side effects (from clinical data) and user-reported satisfaction and concerns (from sentiment analysis). By examining both datasets in parallel, insights into patterns become apparent that may not be visible in either dataset alone. For example, sentiment analysis on the general comments of the drug are heavily positive, despite how often users obtain a critical condition as an adverse effect. Of course, the sample size of the reviews is small and obtaining much larger samples would better represent the general publics view on the drug for sentiment analysis. 

My dual-dataset approach hopefully enables you, the reader, to obtain a comprehensive understanding of Ozempic’s impact, while also illustrating how clinical and real-world perspectives align or diverge.





### Side Note:

Below is an furhter analysis of the other key column in openFDA's dataset that I chose to use:

- safetyreportid: A unique identifier for each safety report in the openFDA dataset. This ID is critical for linking and cross-referencing individual reports.
- seriousnessdeath: Specifies if the adverse event resulted in death. Useful in understanding the mortality risk associated with Ozempic
- reportercountry: Indicates the country of the reporter (e.g., the patient, healthcare provider). Useful if seeking to identify any country-specific trends or potential differences in adverse events across regions.
- reporterqualification: Specifies the qualification of the person who reported the event (e.g., physician, pharmacist). Useful if you want to explore if the reported adverse affects of Ozempic are captured more in-depth by a certain party.
- patient_age_unit: Unit of age measurement (e.g., years, months). This field ensures that patient age data is consistently interpreted.
- drug_name: The name of the drug involved in the adverse event report (in my openFDA dataset, its only Ozempic).
- drug_characterization: Characterizes the drug's role in the adverse event. This field helps differentiate between adverse reactions directly attributed to Ozempic and those that might be related to other medications taken concurrently. There are 3 characterizations I use: 
  1. Causal (suspected cause of the event).
  2. Concurrent (taken concurrently but not suspected of causing the event).
  3. Synergistic (may have interacted with the causal drug).
- drug_indication: Indicates the primary reason for prescribing the drug, typically the patient’s diagnosis (e.g., Type 2 diabetes). Useful as it can provide insights into whether certain health conditions increase the likelihood of specific adverse events.

> Yes, this is probably WAY too many features for a dataset, I know that. I also know that I won't be doing numerous or intense data exploration using my datasets, as this is a BLOG READ and not my conclusive project for an employer. I added these since openFDA's dataset IS VERY CONFUSING so if there was someone who was interested in doing this for a resume booster, they could see what they COULD do with this side of the dataset.  









### REST APIs

Imagine EST APIs <strong>dominate</strong> the API usage space. According to Postman, the number one tool for API management and design, [89%](https://analyzingalpha.com/api-statistics) of respondents in their 2022 survey reported their API architecture to be specifically designed around REST APIs.

RE like ```GET```, ```POST```, ```PUT```, and ```DELETE```. For future coding, remember that REST APIs are stateless. Each client request is independent 


Another important JSON or XML.

## Fdds


## Step 4: Putting it all together

\output a message that looks similar to this (I stylized mine for fun):

{% include framework/shortcodes/figure.html src="/assets/files/Zoho-Message-Card-Image.webp" title="The message card is scrollable, showing you the hourly temps as well" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


## Conclusion


Ye didn't discuss so I hope you take time to research more about this side of programming. 

### Additional Resources
