---
layout: post
title: "Analysis on FDA Drug Ozempic"
date: 2024-10-26T09:49:03Z
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

It's litle wonder why so many have flooded clinics, hopeing to pull favors from doctors to get a prescription for the drug. And when not possible, many purchase for off-brand or out-of-country drug alternatives, hoping to regain their past body types again. I would know, as I have family members who have switched to Ozempic or similar drugs for managing their diabetes and also hope to cut a few pounds on the side. And believe me readers, the drug is not so miraculous when you analyze the adverse effects it causes users. 

It is because of this curiosity that I have created 2 seperate but important datasets for those who wish to explore the adverse effects of Ozempic. I will explain my reasoning in keeping these datasets seperate further into this blog.

My dataset project explores clinical and real-world data related to Ozempic's side effects and patient-reported experiences. By comparing clinical adverse event reports with user reviews, my datasets project aims to provide a clearer picture of the medication's impact on diverse patient demographics and highlight discrepancies or consistencies between clinical trials and everyday usage.

## Zoho Deluge and REST API

### Zoho Deluge:

Delugng:
- Built-in functions and wrappers tailored for Zoho applications
- No egrated query capabilities

### REST APIs

Imagine EST APIs <strong>dominate</strong> the API usage space. According to Postman, the number one tool for API management and design, [89%](https://analyzingalpha.com/api-statistics) of respondents in their 2022 survey reported their API architecture to be specifically designed around REST APIs.

RE like ```GET```, ```POST```, ```PUT```, and ```DELETE```. For future coding, remember that REST APIs are stateless. Each client request is independent 


Another important JSON or XML.

## Fdds


Data Reliability and Consistency: Clinical data from openFDA is highly regulated, while review data from Drugs.com is self-reported and unverified. Mixing these two sources without careful consideration could lead to biases or misinterpretations.

Sample Bias: Patients who report adverse events to the FDA are often different from those who leave reviews on Drugs.com. Those who leave reviews may tend to report experiences based on satisfaction or frustration, which could bias sentiment analysis results.

Integration Difficulty: Clinical data and review data have few shared identifiers. Without a unique key like a report ID, merging directly is challenging and could lead to inaccuracies if done incorrectly.

Mixed Purpose: Clinical adverse event data is collected for regulatory oversight and safety monitoring, whereas reviews are often written for other patients or general audiences, which might lead to fundamental differences in data scope and purpose.


### User Story
Assume  selects from your provided lists of common weather data points:

- Temperature: Recorded temperature (°F)

### Research
briefly discuss my research process so you understand how the code will function further in the walkthrough:

1. We are pulling current data from external sources into Zoho Cliq, so REST APIs will be useful here
2. Open Metro has a free API that can obtain our data desired, but it requires both Longitude (-82.0200) and Latitude (33.5000)

## Project Setup

First, we need

###  Keys

For ed objects, please feel free to skip this section.

simple enough without requiring any in-depth transformation 

### Design our Zoho

There are plenty of documentation detailing how to access the "Command" build-out tool from the "Bots & Tools" link in Zoho Cliq. Therefore I will not 

## Step 1: invokeURL

With both our API Url createed and our 

To begin, we 

### Is requesting?

Sadly, no. Other this process for any other parameters desired.

## Step 2: Wind

You may have no

## Step 3: Hour

We have now com

## Step 4: Putting it all together

\output a message that looks similar to this (I stylized mine for fun):

{% include framework/shortcodes/figure.html src="/assets/files/Zoho-Message-Card-Image.webp" title="The message card is scrollable, showing you the hourly temps as well" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


## Conclusion


Ye didn't discuss so I hope you take time to research more about this side of programming. 

### Additional Resources
