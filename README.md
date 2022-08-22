# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Web APIs & NLP

### Contents:
- [Introduction and Problem Statement](#Introduction-and-Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Conclusions and Recommendations](#Conclusions-and-Recommendations)

### Introduction and Problem Statement

Reddit is a social network of threads organized by subject and separated by interests. Users can post to these individual subreddits and interact with other members who share similar interests. The popularity of Reddit means there is a wealth of data that continues to grow daily, making it the perfect platform to aggregate data and analyze these data sets to note trends or make predictions. The ability to predict the subreddit from which a post originated is an example of a classification model that could be useful to members of the Reddit team who are responsible for the website's oversight. 

**The goal of this project is to create an NLP model that accurately predicts from which subreddit a given post originated.** This will be done by collecting posts from the subreddits [r/LifeProTips](https://www.reddit.com/r/LifeProTips/) and [r/ShittyLifeProTips](https://www.reddit.com/r/ShittyLifeProTips/) and classify these posts based on their content. 

More specifically, these subreddits include tips that can be used to improve one's life or, on the other hand, "tips" that mock the idea! r/LifeProTips is full of helpful advice from reddit users, while r/ShittyLifeProTips is full of silly content and advice that simply doesn't make sense. So, **Can we use NLP to discern a good/helpful tip from a bad one?**

#### Executive Summary

The data for this project was collected from each subreddit through [Pushshift's](https://github.com/pushshift/api) API. In order to create a robust model, approximately 10,000 posts from each subreddit were used for classification. Pushshift scrapes a lot of information for each post, however posts to these specific subreddits contain the bulk of their message in the post's title. For this reason, the only data we save for analysis are the subreddit name and the title of each post. The data, including original subreddit, original title post, word count of original title post, and cleaned title post (without stop words) can be found [here](https://git.generalassemb.ly/jserigano4/project-3/tree/master/data).

Before we can classify these posts, some data cleaning and preprocessing are required. This includes removing any duplicate posts or null values, and removing common stop words from each title.

Preliminary analysis shows that our subreddit posts are fairly similar to one another. The average word count is 19 words per title for both subreddits.

# ![](https://github.com/jserigano4/NLP_reddit_classification/blob/main/figures/word_count.png)

Additionally, these subreddits share a lot of the most common bigrams as well.

# ![](https://github.com/jserigano4/NLP_reddit_classification/blob/main/figures/LifeProTips_cleaned.png)

# ![](https://github.com/jserigano4/NLP_reddit_classification/blob/main/figures/ShittyLifeProTips_cleaned.png)

Multiple classification models were then developed in order to determine which variation of vectorizer and classifier produced the most accurate model predictions.

The vectorizers included:
- CountVectorizer()
- TfidfVectorizer()

The classifiers included:
- LogisticRegression()
- KNeighborsClassifier()
- MultinomialNB()
- RandomForestClassifier()
- AdaBoostClassifier()
- GradientBoostingClassifier()

All together, 12 different combinations were evaluated along with a parameter search for each model. In the end, a Logistic Regression model with L2 (Ridge) regularization and TfidfVectorizer with no preprocessing of the titles (i.e., no lemmatizing or stemming of words). This model accurately predicted the subreddit classification of 75.7% of the test data based on our input features. It should be noted that the scores for our next best-fitting models are very similar to the best model. These models are a Multinomial Naive Bayes model with TfidfVectorizer and another Logistic Regression model with L2 regularization and CountVectorizer. Additionally, all models tested here outperformed the baseline null model (50.4% accuracy), and train and test scores for each model were very similar, indicating that the models were not overfit.

An accuracy of 75.7% is not very high but this is also not very surprising. As noted earlier, these data sets were very similar to each other, right down to the average word count per title. Additionally, these data sets also had very similar word usage. With no discerning qualitites to either data set, an accuracy of 75.7% is perhaps as high as one can achieve!

## Recommendations

Although the models presented here do not have a significantly high accuracy, there are still key takeaways and there is still room for improvement. Below are some key takeaways and recommendations that can be used for future analysis.
- Two of the three highest performing models leverage logistic regression as the classifier.
- A larger data set (> 10,000 posts per subreddit) could possibly lead to better results. 
- Developing an ensemble model, such as a VotingClassifier or stacked ensemble model, would combine different models and produce better results. 
