---
layout: post
title: "Ozempic Under the Microscope: A Deep Dive into Its Controversial Effects"
date: 2024-12-02T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Drugs", "EDA"]
description: Explore the hidden truths and potential dangers of Ozempic in comparison to online review as we dissect the drugs controversial effects in this in-depth analysis.
thumbnail: "/assets/images/gen/blog/blog-22-thumbnail.png"
image: "/assets/images/gen/blog/blog-22.png"
comments: false

---

## openFDA and Ozempic

There has been a lot of talk about drug problems. And no, I'm not referring to the ones dicussed in intense immigration arguements. I'm talking about the drugs you get from the white angel of death, doctors! Or at least that's the opinion of some who have followed onto a recent popularity trend of using Ozempic. 

Ozempic is used for managing Type 2 diabetes but that is not what is captivating the attention of millions of Americans. It's Ozempics secondary, off-label effect, the drugs ability to help individuals lose weight. And lose weight you do, at an insane amount. Reports from users taking Ozempic have sworn the drug has caused them to lose to as much as 30 pounds in a month. In comparison, exercise and a clean diet, on average, nets you a loss of about 3-5 pounds per month.

It's litle wonder why so many have flooded clinics, hoping to pull favors to get a prescription for the drug. And when not possible, many purchase off-brand or out-of-country drug alternatives, hoping to regain their past body types again. I would know, as I have family members who have switched to Ozempic or similar drugs for managing their diabetes and also hope to cut a few pounds on the side. And believe me readers, the drug is not so miraculous when you analyze the adverse effects it causes users. 

## My Inference

Of course, what you hear on the news, ads, social media differs from the data obtained by the FDA. But most interesting to me is that despite the drugs harmful effects, overall patient perspective is generally postiive of the drug. In my own exploration of the many posted reviews, people love the postiive effects of the drug, even when there is physical harm done to their bodies. That is fascinating to me and it is that discrepancy that I will explore as my primary inference in this blog post.

### Top 10 Most Common Side Effects Ozempic
To start explaining my inference, I want to show the 10 most frequently reported side effects for Ozempic. This bar chart highlights these side effects and their counts, offering a clear view of the most common patient experiences.

My Key Findings
- Prevalence of Gastrointestinal Issues: Side effects like nausea, vomiting, and diarrhea appear most frequently, suggesting that gastrointestinal reactions are a significant aspect of Ozempic’s adverse effect.
- Other Common Reactions: Fatigue and headache were also frequently reported, indicating that Ozempic’s effects are not limited to the digestive system but can impact overall energy and comfort.

So Ozempic causes life hampering effects for individuals who takke the medication. And from the sheer count towards nausea we can see this side effect is not unique to specific individuals. Add onto that vomiting and diahoea...well taking Ozempic sounds more like neeidng to eat moldy bread each day.


{% include framework/shortcodes/figure.html src="/assets/files/Top_10_Side_Effects_Ozempic.png" title="Top_10_Side_Effects_Ozempic" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}

Note: I have hidden my code snippets in various accordians to preserve the design of my blog (as seen below). Simply click on the accordian to see my code.  

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
The next piece of context towards understanding the negative effects of Ozempic is how often complications from taking the drug turn into serious medical issues. For that I created a pie chart to show this distribution. And please be aware, the FDA labels a complication as serious when it causes: the patient to end up in the emergency room, causes a permanent disability, or has the individual in a life threatening state.

Key Insights
- High Proportion of Serious Cases: A significant portion of reported events from openFDA database on Ozempic are classified as "Serious," indicating that while Ozempic may be effective for its intended uses, it also comes with high risks. 

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
Onto our next stop in understanding Ozempics complications is analyzing the reported ages of individuals who experienced complications with the medication. I have created a histogram shows the age distribution of patients who reported adverse events related to Ozempic, with the mean and median ages marked for reference.

Key Observations
- Diverse Age Range: The distribution shows that adverse events were reported across a broad range of ages, with the mean age slightly above the median, suggesting a slight skew toward older age groups.
- The majority of reports on the adverse effects of Ozempic come from individuals between 40 - 80 years old

Note that the reported complications are not exclusive to the elderly. Even adults in their mid 40s have more often reported adverse effects from using the drug.

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
So we saw that Ozempic can cause pretty bad effects for those who use it. But how do users of the drug feel about it? Pretty bad I assume since if your feeling nauseaus, vomiting, fatigues, and have a strong liklihood of ending up in an emergency room or obtaining a chronic disability. Well in reality...these next bits of analysis may shock you.

Below is a chart that shows the average sentiment score associated with different side effects as reported in Ozempic user reviews on Drugs.com. Sentiment scores range from -1 (negative) to +1 (positive), which reflect the overall tone of users’ experiences with each side effect.

Key Observations
- Negative Sentiment for Fatigue: Reviews that discussed having fatigue from Ozempic were MUCH more negative than reviews without the mentioning of Fatigue.
- Positive Sentiments for Other Effects: If a review did not mention faitgue, it was generally quite positive. Even when mentioning vomiting or pancreatis, reviewers still felt like Ozempic was amazing for them.


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
Upon seeing how happy people's reviews were of Ozempic (despite the very harmful effects it caused them), I wanted to check the overall sentiment towards Ozempic. The bar chart below illustrates the overall sentiment distribution among Ozempic user reviews, classified into positive, neutral, and negative categories.

Key Takeaways
- Notable Negative Sentiment: A sizeable portion of reviews fall into the negative category, reflecting dissatisfaction potentially linked to side effects or unmet expectations.
- Overwhelming Positive Sentinment: An massize amount of reviews had positive sentiments towards Ozempic, depite how serious the complications are from a negative medicinal reaction.

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
So far I have provided, hopefully, interesting insight on my inference of people's positive persepctive towards Ozempic despite its harmful effects. This next graph categorizes Ozempic user reviews into topics and displays the distribution of sentiments (positive, neutral, and negative) within each. From this I wanted to see what topics were the most least positive when discussing Ozempic. 

Key Takeaways
- Negative Sentiment in Side Effects: The topic "Side Effects & Diabetes" has a higher proportion of negative sentiment, which aligns with previous observations on common side effects and their impact on patient satisfaction. Of course, some of the positive sentiments could be due to the side effect of weight loss that some would consider a positive effect.
- Stronger postive sentiment for Ozempic when the drug is used predominately for diabetes managment. This is contrasted with the slighly more mixed sentiment when the drug was discussed as only being used for weight loss.


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
As a recap, my EDA used two distinct but complementary datasets to examine Ozempic’s impact from both clinical and real-world perspectives. While these datasets are not merged, they offer unique insights when analyzed in parallel, allowing for a holistic view of Ozempics adverse events and the positve patient perspective towards the drug.

1. Complementary Insights

    In the social craze of obtaining the media hyped wonder drug, my exploration shows you, the reader, that Ozempic is well liked among those using it. Despite its negative effects, patients find the value of losing weight greater than fatigue, vomiting, and moments of pancreatis. That tells us that weight loss is truly a wonderdrug that so many craze today. And, even more importantly, that the most postiive reviews come from reviewers who are getting their diabetes under control. While it is unfortunate that so many are making Ozempic limited for those who need it, let us realize that those who appreciate the drug the most are the ones with the greatest medical need. It gives those struggling with diabetes another shot at life, which is worth the risks and pains the drug causes.
   
    NOTE: One weakness of this exploration is the small sample size of the reviews on Drugs.com as obtaining much larger samples would better represent the general publics view on the drug for sentiment analysis.

Stay safe and stay healthy!

### Resources Links:

- openFDA API Documentation [Link](https://open.fda.gov/apis/): Comprehensive guide on how to use the openFDA API to gather and analyze drug data.
- Drugs.com Ozempic Reviews [Link](https://www.drugs.com/comments/semaglutide/ozempic.html): For accessing user reviews to understand public sentiment and patient experiences with Ozempic.
- TextBlob Documentation [Link](https://textblob.readthedocs.io/en/dev/): Learn how to conduct basic sentiment analysis using TextBlob.
- Latent Dirichlet Allocation (LDA) with Scikit-Learn [Link](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html): A guide on using LDA for topic modeling, as applied in this analysis to categorize user reviews.

Link to my code: [Link](https://github.com/garcia57/Adverse-Effects-openFDA/tree/main)