# Twitter Discourse on COVID-19 Vaccines and Vaccination

## Background
Although the increasing body of studies has contributed a great number of social media datasets on COVID-related discourse from the perspective of fake news detection, the content of the posts, especially how discourse was framed and spread by different users, are rarely examined thoroughly. In the paper: [working title: Opinions, Positions, and Information Dissemination: Twitter Discourse on COVID-19 Vaccines and Vaccination], we created a new Twitter dataset of German-language discourse on COVID-19 vaccines and vaccination. We endeavor to make following contributions: (1) detecting bias in different communities as well as establishing transparent and reliable coding rules, and (2) understanding the corresponding discourse by contextualizing the tweeting patterns of different communities.

## Dataset
The dataset (dataset.csv) contains 111,482 unique retweeted tweets (1,098,058 retweets) posted by two polarized communities: conservative and right-leaning community A (85,791 retweeted tweets of 855,254 retweets) and liberal and left-leaning community B (31,050 retweeted tweets of 242,804 retweets). Under the active learning approach, we annotated 2,192 seed tweets (annotated_seed.csv) with labels for users' stances and discursive frames and 862 tweets (annotated_exp.csv) with corrected/confirmed discursive frames that are most probably to improve classifier training. 


## Data Collection  
The data collection follows two steps. Via Twitter API, we first queried tweets based on a keyword list containing the 28 most important keywords (initial_keywords.csv) of COVID-19 related discourse provided by a domain expert of Querdenken movement and fact-checking. The search resulted 132,477 retweeted German-language tweets (original tweets of the retweets) in the period from March 11, 2020, to December 31, 2021. Next, we reviewed the top 5% of most-mentioned hashtags among all 36,367 hashtags. After getting a general overview of words related to vaccines and vaccination, we searched for “impf,” “pharma,”  “biontech,” “moderna,” “pfizer,” “johnson,” “astrazeneca,” and “mrna” through the list of the 36,367 hashtags and excluding hashtags with a focus on general anti-pandemic policies, political parties founded during the pandemic, and the hashtags that attacked pro-vaxxers, associated with protests, or aimed to initiate discussions neutrally. Finally, we obtained 247 valid hashtags with negative connotations linked to COVID-19 vaccines and vaccination as keywords (final_keywords.csv) to create the search query. We collected 1,727,358 German-language tweets (1,104,650 retweets and 114,088 retweeted original tweets, of which 90 tweets were not accessible or available) in the period from March 11, 2020, to December 31, 2021, covering all the crucial issues related to the COVID-19 since the WHO declared the outbreak as a pandemic.  


## Data Curation
We explored the initial dataset through network analysis and text analysis. Based on the results of community detection, two siginificant communities are found. To optimize the scope of the analysis, we excluded the (re)tweets from the rest of the users. The **final dataset** consists of 111,482 unique retweeted tweets (1,098,058 retweets) exclusiviely posted by conservative and right-leaning community A (85,791 retweeted tweets of 855,254 retweets) and liberal and left-leaning community B (31,050 retweeted tweets of 242,804 retweets).


## Sampling and Annotation - seed dataset
To ensure the representativeness of the dataset, we constructed a seed datasetby selecting the most retweeted tweets and randomly sampling other tweets for the two communities, respectively. There are eight different subsets in the seed dataset (n = 2,192, 2.0% of the whole dataset):
1. 1% most influential in-group tweets of community B (n = 278)
2. 1% random sample of the rest in-group tweets of community B (n = 275)
3. 1% most influential in-group tweets of community A (n = 818)
4. random sample of the rest in-group tweets of community A (n = 818)
5. 1% most influential between-group tweets taken from community A introduced to community B (n = 32)
6. 1% random sample of the rest between-group tweets taken from community A introduced to community B (n = 372)
7. 1% most influential between-group tweets taken from community B introduced to community A (n = 39)
8. 1% random sample of the rest between-group tweets taken from community B introduced to community A (n = 231). 

Since users can also spread in-group retweets from another group through between-group retweets, subsets (1), (2), (3), and (4) are mutually exclusive. Subsets (5) and (7) are included by subsets (3) and (1), respectively. As subsets (6) and (8) are randomly selected and introduced, some tweets are already in the dataset. Thus, the sample size is not directly equal to the sum of the samples from the eight subsets. 
 
We manually annotated the discursive frames in the tweets of the seed dataset based on the categories: (1) effectiveness of vaccines, (2) vaccine side effects, risks, damages, deaths, (3) vaccine distributions, privileges, exports, (4) mandatory vaccination and the potential consequences, (5) politics, regulations, governments, parties, politicians, and institutions (6) pharmaceuticals, (7) conspiracy theories, (8) children-related topics, (9) other. Each tweet can have one or multiple labels. Meanwhile, we labeled the stances of the tweets to verify the positionality of involved users. The stances towards COVID-19 vaccines and vaccination were classified into four mutually exclusive classes (1) pro, (2) anti, (3) neutral, (4) sarcasm by pro-vaxxers, and (5) sarcasm by anti-vaxxers. While finding out discursive frames is a multi-label (i.e., there are more than two categories), multi-class classification task (i.e., each tweet can belong to multiple categories), the stance identification involves a multi-label classification task.

## Sampling and Annotation - active learning
Based on the concept of active learning, we assessed classifiers on a randomly selected labeled collection of items representing 10% of the (growing) dataset at each iteration. We conducted three iterations to annotate the dataset, in which 862 tweets were annotated according to the discursive frames.

## Data Structure - the dataset (retweets)
1. time: Format ("yyy-mm-ddThh:mm:ss.000Z").
2. id: Long (numerical id of the retweet).
3. author_id: Long (numerical id of the user who posted the retweet).
4. ref_id: Long (original id of the retweeted tweet).
5. source_m: String (community of the user who retweet, based on community detection algorithm - community [A/B]).
6. target_m: String (community of the user who is retweeted, based on community detection algorithm - community [A/B]).

## Data Structure - seed dataset (retweeted tweets) with annotation (stances and discursive frames)
1. ref_id: Long (original id of the retweeted tweet).
2. stance: Int (0: anti-vax, 1: pro-vax, 2: sarcasm by pro-vaxxers, 3: sarcasm by anti-vaxxers).
3. effectiveneess: Int (0: related, 1: unrelated).
4. risks: Int (0: related, 1: unrelated).
5. distribution: Int (0: related, 1: unrelated).
6. mandatory: Int (0: related, 1: unrelated).
7. politics: Int (0: related, 1: unrelated).
8. pharma: Int (0: related, 1: unrelated).
9. ct: Int (conspiracy theories/ 0: related, 1: unrelated).
10. child: Int (0: related, 1: unrelated).
11. other: String (potential category which could be established later).
12. other_merged: String (merged potential category).
13. cate_in: String (whether the post is sampled as in-group retweet retweeted by a user from community [A/B], whether it is a influential post [A_inf/B_inf], or whether the retweet, sampled in other subset, is exclusively a between-group retweet regarding the whole dataset, which is not retweeted within users from the same community [between]).
14. cate_bt: String (whether the post is sampled as between-group tweet retweeted by user from community [A/B], whether it is a influential post [AB_inf/BA_inf], or whether the retweet, sampled in other subset, is exclusively an in-group tweet that is not retweeted between users of different communities [within]).

## Data Structure - expanded dataset (retweeted tweets) with annotation (discursive frames)
1. ref_id: Long (original id of the retweeted tweet).
2. it_round: Int (annotated at the n-th iteration).
3. effectiveneess: Int (0: related, 1: unrelated).
4. risks: Int (0: related, 1: unrelated).
5. distribution: Int (0: related, 1: unrelated).
6. mandatory: Int (0: related, 1: unrelated).
7. politics: Int (0: related, 1: unrelated).
8. pharma: Int (0: related, 1: unrelated).
9. ct: Int (conspiracy theories/ 0: related, 1: unrelated).
10. child: Int (0: related, 1: unrelated).

## Code Reference
- Data Exploration / word embedding: https://chriskhanhtran.github.io/_posts/2020-02-01-cnn-sentence-classification/
- Methods / TextCNN: https://github.com/FUB-HCC/seminar_critical-social-media-analysis
- Results / topic modeling: https://towardsdatascience.com/twitter-topic-modeling-e0e3315b12e2

