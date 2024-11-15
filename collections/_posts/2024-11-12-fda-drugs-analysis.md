---
layout: post
title: "Analysis on FDA Drug Ozempic"
date: 2024-11-12T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Drugs", "EDA"]
description: This blog post will disucss my curated dataset from openFDA and webscrapped reviews on Ozempic from drugs.com.
thumbnail: "/assets/images/gen/blog/blog-22-thumbnail.png"
image: "/assets/images/gen/blog/blog-22.png"
comments: false

---

## openFDA and Ozempic

There has been a lot of talk about drug problems. And no, I'm not referring to the ones dicussed in intense immigration arguements. I'm talking about the drugs you get from the white angel of death, doctors! Or at least that's the opinion of some who have followed onto a recent popularity trend of using Ozempic. 

Ozempic is used for managing Type 2 diabetes but that is not what is captivating the attention of millions of Americans. It's Ozempics secondary, off-label effect, the drugs ability to help individuals lose weight. And lose weight you do, at an insane amount. Reports from users taking Ozempic have sworn the drug has caused them to lose to as much as 30 pounds in a month. In comparison, exercise and a clean diet, on average, nets you a loss of about 3-5 pounds per month.

It's litle wonder why so many have flooded clinics, hopeing to pull favors from doctors to get a prescription for the drug. And when not possible, many purchase off-brand or out-of-country drug alternatives, hoping to regain their past body types again. I would know, as I have family members who have switched to Ozempic or similar drugs for managing their diabetes and also hope to cut a few pounds on the side. And believe me readers, the drug is not so miraculous when you analyze the adverse effects it causes users. Of course, what you hear on the news, ads, social media differs from the data obtained by the FDA, which I will demonstrate as well. 

## Motivating Question

Its this disparity in perception that fascinates and motivates me to do this project. My dataset project seeks to answer the following: What are the most common and severe side effects associated with Ozempic, and how does this drugs perception differ between obtained adverse effect reports and real-world patient reviews?

I will identify the following:
- Top 10 most reported side effects
- Severity distribution of reports obtained by the FDA
- Age distribution of people reporting adverse effects from using Ozempic
- Average sentiment by most commonly reported side effect in Drugs.com reviews of the drug
- Overall Sentinment distribution across all Drugs.com review
- Topic vs Sentiment Distribution for Ozempic User Reviews 

These graphs will explore the commonalities / discrepancies clinical and real-world data related to Ozempic's side effects and patient-reported experiences. By comparing clinical adverse event reports with user reviews, my datasets project aims to provide a clearer picture of the medication's impact on patient perceptions.

## Ethical Considerations and Good Scraping Practices

Data ethics and responsible data gathering were prioritized in this project. Here’s how I ensured ethical compliance:

- Data Source Verification: The data comes from publicly accessible resources—openFDA and Drugs.com, both of which were selected due to their allowance for data usage in research and analytical purposes. The datasets were curated to ensure its access aligns with both platform's guidelines.
- Personal Identifiability: The datasources were screened to ensure excluding any personally identifiable information (PII), protecting patient privacy.
- Respectful API Use: Data was collected using openFDA’s official API, which explicitly permits data extraction within reasonable limits. Rate limiting and data retrieval intervals were implemented to respect server load and prevent excessive requests.

## Steps for Data Collection

You can follow these steps to replicate a similar data gathering project:

1. Define your Data's Needs: Identify relevant fields, such as adverse reactions, patient demographics, and reported severity, to answer your motivating question.
2. Access openFDA Data: Register for an API key (if applicable) from openFDA and use it to request data on Ozempic’s adverse events.
3. Scrape Drugs.com User Reviews: Use a web scraping tool (I used Python's BeautifulSoup package) to extract user reviews, paying attention to rate limits and ethical scraping guidelines.
4. Store and Clean Data: Store data in CSV or JSON format, then clean it by handling missing values, categorizing reactions, and setting up sentiment analysis for user reviews.


## Key Metrics and Counts

Total Sample Size: Over 400 individual records of adverse events related to Ozempic from openFDA.

Counts of Categorical Variables:

- Adverse Reaction Type (reaction_meddra): Provides counts for each adverse reaction type, such as nausea, fatigue, and injection site pain, etc.
- Seriousness (serious): Categorizes cases as serious (1) or non-serious(0), with counts showing the proportion of severe cases.
- Patient Sex (patient_sex): Number of cases by patient sex by Male (1), Female (2), or Unspecified (3) .
- Drug Administration Route (drug_admin_route): Breakdown of the methods of administration, such as subcutaneous injection or other routes.

Numerical Summaries of Key Numeric Variables:

- Patient Age (patient_age): Summary statistics (ex: mean, median, minimum, maximum) to understand the age range of affected patients.
- Received Date (receivedate): The date when the adverse event was reported and received by the FDA. Useful for providing a timeline on adverse event reports and identifying trends/spikes over time (e.g., seasonal/social media patterns, post-approval phases).


## Analytical Analysisz

In this section, I explore the dataset and some of the findings made with it:


### Top 10 Most Common Side Effects Ozempic
Below is the top 10 most frequently reported side effects for Ozempic. This bar chart highlights these side effects and their counts, offering a clear view of the most common patient experiences.

My Key Findings
- Prevalence of Gastrointestinal Issues: Side effects like nausea, vomiting, and diarrhea appear most frequently, suggesting that gastrointestinal reactions are a significant aspect of Ozempic’s adverse effect.
- Other Common Reactions: Fatigue and headache were also frequently reported, indicating that Ozempic’s effects are not limited to the digestive system but can impact overall energy and comfort.


{% include framework/shortcodes/figure.html src="/assets/files/Top_10_Side_Effects_Ozempic.png" title="Top_10_Side_Effects_Ozempic" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    # Recalculate the top 10 most frequently reported side effects for the cleaned dataset
    filtered_side_effect_counts = df['reaction_meddra'].value_counts().head(10)

    # Plotting the revised side effect frequency chart
    plt.figure(figsize=(10, 6))
    filtered_side_effect_counts.plot(kind='bar')
    plt.title("Top 10 Most Frequently Reported Side Effects for Ozempic (Filtered)")
    plt.xlabel("Side Effect")
    plt.ylabel("Count")
    plt.xticks(rotation=45)
    plt.show()
  </code></pre>

</details>

### Severity Distribution of Adverse Events
I created a pie chart to show the distribution of adverse events that resulted in obtaining a serious medical complication for Ozempic. 

Key Insights
- High Proportion of Serious Cases: A significant portion of reported events from openFDA database on Ozempic are classified as "Serious," indicating that while Ozempic may be effective for its intended uses, it also comes with high risks from complications.

{% include framework/shortcodes/figure.html src="/assets/files/Severity-Dist-Ozempic.png" title="Severity Distribution -Ozempic" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    # Calculating the proportion of serious vs. non-serious cases
    severity_counts = df['serious'].value_counts()

    # Plotting the severity distribution chart as a pie chart
    plt.figure(figsize=(8, 8))
    plt.pie(severity_counts, labels=['Serious', 'Non-Serious'], autopct='%1.1f%%', startangle=140)
    plt.title("Severity Distribution of Adverse Events")
    plt.show()
  </code></pre>

</details>


### Age Distribution of Patients Reporting Adverse Events
The histogram below shows the age distribution of patients who reported adverse events related to Ozempic, with the mean and median ages marked for reference.

Key Observations
- Diverse Age Range: The distribution shows that adverse events were reported across a broad range of ages, with the mean age slightly above the median, suggesting a slight skew toward older age groups.
- The majority of reports on the adverse effects of Ozempic come from individuals around 50 years old

{% include framework/shortcodes/figure.html src="/assets/files/Age-dis-Ozempic.png" title="The message card is scrollable, showing you the hourly temps as well" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    # Convert 'patient_age' to numeric to handle any non-numeric entries
    df['patient_age'] = pd.to_numeric(df['patient_age'], errors='coerce')

    # Filter ages to a realistic range (1 to 100 years)
    ages = df[(df['patient_age'] >= 1) & (df['patient_age'] <= 100)]['patient_age'].dropna()

    # Recalculate the mean age after filtering
    mean_age = ages.mean()

    # Create the histogram plot with mean and median markers
    plt.figure(figsize=(8, 6))
    plt.hist(ages, bins=30, density=True, alpha=0.3, color='blue', edgecolor='black')
    plt.axvline(np.median(ages), color='green', linestyle='-', label=f'Median Age: {np.median(ages):.1f}')
    plt.axvline(mean_age, color='red', linestyle='--', label=f'Mean Age: {mean_age:.1f}')

    # Adding labels, title, and legend
    plt.title("Age Distribution of Patients Reporting Adverse Events for Ozempic")
    plt.xlabel("Age (Years)")
    plt.ylabel("Density")
    plt.legend()

    plt.show()
  </code></pre>

</details>


### Avg Sentinment by Reported Side Effect
The chart below shows the average sentiment score associated with different side effects as reported in Ozempic user reviews on Drugs.com

Sentiment scores range from -1 (negative) to +1 (positive), which reflect the overall tone of users’ experiences with each side effect.

Key Observations
- Negative Sentiment for Nausea and Diarrhea: Side effects like nausea and diarrhea are associated with the lowest sentiment scores, indicating a generally negative experience among users who reported these symptoms in their review.
- Mixed Sentiments for Other Effects: Fatigue and headache also show lower sentiment scores, though they are less negative than nausea or diarrhea. Appetite suppression has a more neutral score, which may be due to how some users might view a disinterest to eat as a postiive side effect for weight management.


{% include framework/shortcodes/figure.html src="/assets/files/avg-sentiment-sideeffect-ozempic.png" title="Avg Sentinment by Reported Side Effect" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    import pandas as pd
    from textblob import TextBlob
    import matplotlib.pyplot as plt

    # Sample reviews and sentiment analysis
    reviews_df = pd.read_csv("ozempic_reviews.csv")

    # Conduct sentiment analysis on each review
    reviews_df['sentiment'] = reviews_df['review_text'].apply(lambda x: TextBlob(x).sentiment.polarity)

    # Define common side effects and associated keywords
    side_effects_keywords = {
        "Nausea": ["nausea", "vomit", "upset stomach"],
        "Fatigue": ["fatigue", "tired", "exhausted"],
        "Headache": ["headache", "migraine"],
        "Appetite Suppression": ["appetite", "not hungry", "reduced appetite"],
        "Diarrhoea":["diarrhoea","loose stool"],
        "Pancreatitis":["pancreatitis"]
    }

    # Created a column to categorize reviews by side effect
    def categorize_side_effect(text):
        for side_effect, keywords in side_effects_keywords.items():
            if any(keyword in text.lower() for keyword in keywords):
                return side_effect
        return "Other"  # Use "Other" for reviews without specified side effects

    reviews_df['side_effect_category'] = reviews_df['review_text'].apply(categorize_side_effect)

    # Calculate average sentiment for each side effect category
    avg_sentiment_by_side_effect = reviews_df.groupby('side_effect_category')['sentiment'].mean()

    # Plot the average sentiment for each side effect
    plt.figure(figsize=(10, 6))
    avg_sentiment_by_side_effect.plot(kind='bar', color='skyblue', edgecolor='black')
    plt.title("Average Sentiment by Reported Side Effect in Ozempic Reviews")
    plt.xlabel("Side Effect Category")
    plt.ylabel("Average Sentiment Score")
    plt.xticks(rotation=45)
    plt.grid(axis='y', linestyle='--', alpha=0.7)
    plt.show()
  </code></pre>

</details>

### Sentiment Distribution of Ozempic User Reviews
The bar chart below illustrates the overall sentiment distribution among Ozempic user reviews, classified into positive, neutral, and negative categories.

Key Takeaways
- Notable Negative Sentiment: A significant portion of reviews fall into the negative category, reflecting dissatisfaction potentially linked to side effects or unmet expectations.
- Overwhelming Positive Sentinment: Many reviewers had positive sentiments towards Ozempic, depite how serious the complications are from a negative medicinal reaction.

{% include framework/shortcodes/figure.html src="/assets/files/sent-dist-allreviews-ozempic.png" title="The message card is scrollable, showing you the hourly temps as well" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    from textblob import TextBlob

    # Apply sentiment analysis to classify each review as positive, neutral, or negative
    def classify_sentiment(text):
        polarity = TextBlob(text).sentiment.polarity
        if polarity > 0:
            return 'Positive'
        elif polarity < 0:
            return 'Negative'
        else:
            return 'Neutral'

    # Add a new column for sentiment classification
    reviews_df['sentiment'] = reviews_df['review_text'].apply(classify_sentiment)

    # Count the occurrences of each sentiment category
    sentiment_counts = reviews_df['sentiment'].value_counts()

    # Plot the sentiment distribution
    plt.figure(figsize=(8, 6))
    sentiment_counts.plot(kind='bar', color=['green', 'gray', 'red'])
    plt.title("Sentiment Distribution of Ozempic User Reviews")
    plt.xlabel("Sentiment")
    plt.ylabel("Frequency")
    plt.xticks(rotation=0)
    plt.show()
  </code></pre>

</details>

### Topic vs Sentiment Distribution for Ozempic User Reviews
My stacked bar chart categorizes Ozempic user reviews into topics and displays the distribution of sentiments (positive, neutral, and negative) within each.

Key Takeaways
- Varied Sentiments by Topic: Topics like "Dosage & Weight Loss" and "Weight Loss & Diabetes Management" show mixed sentiments, possibly reflecting varied user experiences with Ozempic’s effectiveness and side effects in these areas.
- Negative Sentiment in Side Effects: The topic "Side Effects & Diabetes" has a higher proportion of negative sentiment, which aligns with previous observations on common side effects and their impact on patient satisfaction. Of course, some of the positive sentiments could be due to the side effect of weight loss that some would consider a positive effect.


{% include framework/shortcodes/figure.html src="/assets/files/topic-v-sent-ozempic.png" title="Topic vs Sentiment Distribution for Ozempic User Reviews" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


<details>
  <summary>Click to expand code</summary>

  <pre><code class="language-python">
    import matplotlib.pyplot as plt
    from sklearn.feature_extraction.text import CountVectorizer
    from sklearn.decomposition import LatentDirichletAllocation

    # Ensure custom_stopwords is a list, which is compatible with CountVectorizer
    custom_stopwords = ["ozempic", "week", "now", "type", "month", "started", "first", "will", "taking", "took", "told", "going"]

    # Vectorize the text data
    vectorizer = CountVectorizer(stop_words=custom_stopwords, max_df=0.9, min_df=5)
    review_term_matrix = vectorizer.fit_transform(reviews_df['review_text'].astype(str))

    # Initialize and fit the LDA model
    lda_model = LatentDirichletAllocation(n_components=5, random_state=42)
    lda_model.fit(review_term_matrix)

    # Assign topics to each review based on the highest probability of the topic for that review
    # Get the topic distribution for each review
    topic_distribution = lda_model.transform(review_term_matrix)
    reviews_df['topic'] = topic_distribution.argmax(axis=1)

    # Map topic numbers to descriptive labels based on identified themes
    topic_labels = {
        0: "General Experiences",
        1: "Side Effects & Diabetes",
        2: "General Comments",
        3: "Dosage & Weight Loss",
        4: "Weight Loss & Diabetes Management"
    }

    # Apply the topic labels to the new 'topic_label' column
    reviews_df['topic_label'] = reviews_df['topic'].map(topic_labels)

    # Group by the descriptive topic labels and sentiment for a clearer stacked bar chart
    topic_sentiment_counts_labeled = reviews_df.groupby(['topic_label', 'sentiment']).size().unstack().fillna(0)

    # Plotting the labeled stacked bar chart for Topic vs Sentiment Distribution
    # Define colors for each sentiment category
    sentiment_colors = {'Positive': 'green', 'Neutral': 'gray', 'Negative': 'red'}

    # Plotting the labeled stacked bar chart for Topic vs Sentiment Distribution with specified colors
    plt.figure(figsize=(12, 6))
    topic_sentiment_counts_labeled.plot(
        kind='bar', 
        stacked=True, 
        color=[sentiment_colors[col] for col in topic_sentiment_counts_labeled.columns], 
        ax=plt.gca()
    )
    plt.title("Topic vs Sentiment Distribution for Ozempic User Reviews")
    plt.xlabel("Topic")
    plt.ylabel("Review Count")
    plt.xticks(rotation=45)
    plt.legend(title="Sentiment")
    plt.show()
  </code></pre>

</details>


## Conclusive Dataset Overview
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

### Resources Links:

- openFDA API Documentation [Link] (https://open.fda.gov/apis/): Comprehensive guide on how to use the openFDA API to gather and analyze drug data.
- Drugs.com Ozempic Reviews [Link] (https://www.drugs.com/comments/semaglutide/ozempic.html): For accessing user reviews to understand public sentiment and patient experiences with Ozempic.
- TextBlob Documentation [Link] (https://textblob.readthedocs.io/en/dev/): Learn how to conduct basic sentiment analysis using TextBlob.
- Latent Dirichlet Allocation (LDA) with Scikit-Learn [Link] (https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html): A guide on using LDA for topic modeling, as applied in this analysis to categorize user reviews.