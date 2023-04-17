# **NLP Sentiment Analysis**
Sentiment analysis on customer comments with Vader and developing a primitive sentiment analysis method
## *Related work*
As a result of my research on this topic, many have analyzed the consistency of scoring and interpretation. Some used the Vader sentiment analysis available in the NLTK library for this, while others made predictions by text preprocessing and model building.

## *Approach For the Sentiment Analysis of Comments*

In line with my research VADER sentiment analysis works best on short documents, like tweets and sentences, not on large documents. Also, this model doesn't just look at words. Many conjunctions such as capitalizing or lowering the word, punctuation marks in the sentence, and the word "but" also affect the scoring. Therefore, we do not need to do text preprocessing in this part of this project. But there is a  problem here too. Although Vader sentiment analysis seems like a good model, sometimes he can  come across sentences that he cannot decipher, or rather make a wrong analysis.
For example, Let's have two comments. 
- "These trousers are tight. I do not like.”
- "These trousers are tight. I liked it." 

The customer who makes the first comment, give 1 score to the product. Let the other customer give 5 score.
Analysis of these two interpretations according to Vader sentiment analysis respectively:
- *{′𝑛𝑒𝑔′: 0.26, ′𝑛𝑒𝑢′: 0.74, ′𝑝𝑜𝑠′: 0.0, ′𝑐𝑜𝑚𝑝𝑜𝑢𝑛𝑑′: − 0.2755}*
- *{′𝑛𝑒𝑔′: 0.0, ′𝑛𝑒𝑢′: 0.641, ′𝑝𝑜𝑠′: 0.359, ′𝑐𝑜𝑚𝑝𝑜𝑢𝑛𝑑′: 0.4215}*

This was an expected result. Because there are phrases "I like it" and "I don't like it" in sentences. What if these two customers hadn't said whether they liked the product or not? If only they had said that the pants were tight and gave scores. Then the result for these two interpretations would be the same.
- *{′𝑛𝑒𝑔′: 0.0, ′𝑛𝑒𝑢′: 1.0, ′𝑝𝑜𝑠′: 0.0, ′𝑐𝑜𝑚𝑝𝑜𝑢𝑛𝑑′: 0.0}*

As we can see, the analysis result was neutral. Because we do not know whether the tight legs of a trousers are positive or negative. So, this sentiment analysis yielded two inconsistencies. We now know that it is neither positive nor negative. 
So, I had to develop a new method. My current goal is to score each word and add it to the result of my sentiment analysis. But the important point here is to categorize the data. Because there is a difference between tightfitting trousers and tight-fitting clothes. I cannot combine the dress and pants category. For example, the result we will find may say that the word tight is negative in the trousers category, while it can say the opposite in the dress category.
I also need to clean up unnecessary words, numbers, punctuation before scoring words. Because scoring words like “I”, “you”, “do” etc. will not make any sense. My aim is to rate adjectives used for 
dresses that are considered neutral in Vader sentiment analysis. I also must do some pre-cleaning for my algorithm to give a quick result. After cleaning is over, I rate the remaining words in each comment according to their rating. My approach here is, for example, if the word “tight” is used mostly in positive comments, this will show that people are actually using the word “tight” in a positive way. Or if this word is always used in comments that get 1 or 2 ratings, it will mean that the word is negative.
Therefore, I scored the words with the formula I created below.

- 𝑠𝑐𝑜𝑟𝑒 𝑜𝑓 𝑡ℎ𝑒 𝑤𝑜𝑟𝑑  =  ∑(𝑓(𝑖)𝑔(𝑖, 𝑤𝑜𝑟𝑑)) 𝛼 ,𝑖 ∈ 𝐼 

𝑤ℎ𝑒𝑟𝑒 𝑖 = 𝑖𝑛𝑑𝑒𝑥 𝑎𝑛𝑑 𝐼 = 𝑡ℎ𝑒 𝑠𝑒𝑡 𝑜𝑓 𝐼𝑛𝑡𝑒𝑔𝑒𝑟𝑠 𝑓𝑟𝑜𝑚 1 𝑡𝑜 𝑡ℎ𝑒 𝑡𝑜𝑡𝑎𝑙 𝑛𝑢𝑚𝑏𝑒𝑟 𝑜𝑓 𝑟𝑒𝑣𝑖𝑒𝑤𝑠
𝑓(𝑖) = 𝑟𝑎𝑡𝑖𝑛𝑔. 𝑟𝑒𝑣𝑖𝑒𝑤(𝑖) − 3 𝑎𝑛𝑑 𝑔(𝑖, 𝑤𝑜𝑟𝑑) = 𝑡ℎ𝑒 𝑛𝑢𝑚𝑏𝑒𝑟 𝑜𝑓 𝑟𝑒𝑝𝑒𝑡𝑖𝑡𝑖𝑜𝑛𝑠 𝑜𝑓 𝑡ℎ𝑒 𝑤𝑜𝑟𝑑 𝑖𝑛 𝑡ℎ𝑒 "i'th" 𝑟𝑒𝑣𝑖𝑒𝑤

I then summed up the scores I got in other comments for each word. Then I multiply it with an " 𝛼 " to add to the sentiment analysis. The " 𝛼 " is currently unspecified. because e here is a value that will change according to the maximum and minimum word score. After I add this result to the Vader sentiment analysis result, I want to keep my result between 1 and -1.
Then I will combine the result I found with the score I got from the sentiment analysis. Thus, if I find that the words tight are positive, the comment " These trousers are tight." will be positive for us. 
Thus, I will have come down from two inconsistencies to one.

## **Experiment Details:** 

Firstly, In Vader sentiment analysis approach, the result obtained gives a good result using "recommended" and "not recommended" data. such that if we make our compound value for the Vader sentiment analysis result positive if it is greater than 0, and negative if it is less than 0.
When we associate these positive and negative emotions with "recommended" and "not recommended", we get a higher-than-expected result. however, when we looked at the scores and comments, the overlap was less. 

| Table1   |   Negative  |  Positive |
|----------|:-------------:|------:|
| Not Recommend |  423 | 3152 |
| Recommend |    238   |   15849 |

In other words, only 238 of the 16087 suggested items are not consistent with the sentiment of the text. In the other hand, we do not recommend 3152 products that have received a really positive review. Of course, this situation emerges when we separate it into positive and negative. what if we also consider neutral comments? if we make our compound value for the Vader sentiment analysis result positive if it is greater than 0.3, negative if it is less than -0.3, and neutral else: 

| Table2   |   Negative  |  Positive |
|----------|:-------------:|------:|
| Not Recommend |  244 | 2748 |
| Recommend |    97   |   15487 |

In other words, only 97 of the 15584 suggested items are not consistent with the sentiment of the text. On the other hand, we do not recommend 2748 products that have received a really positive review. And about 500 "recommended" products remained neutral. Currently, the data I'm getting is not giving the results I want. therefore, I aim to score words with the formula I mentioned above. Before using our formula, let's do our sentiment analysis based on the ratings over the departments.

| Department Distribution by Sentiment based on the ratings |   Matched  |  Did not match |
|----------|:-------------:|------:|
| Dresses |  2601 | 2770 |
| Bottoms |   1425  |  1759 |
| Tops |  4284 | 4429 |
| Intimate |  654 | 754 |
| Jackets |  388 | 491 |
| Trend |  59 | 48 |


As you can see the results are pretty bad. Now let's apply the formula. This formula may actually yield better results if applied separately for each class, but we will go through all the data and use the cleaned, stemmed data. After doing all the necessary operations, the last step is to choose 𝛼

The result we got is max score 16315 and min score -221. Then the first value for 𝛼 = 1/300∗max_𝑣𝑎𝑙𝑢𝑒

| Department Distribution by Sentiment based on the ratings |   Matched  |  Did not match |
|----------|:-------------:|------:|
| Dresses |  2922 | 2449 |
| Bottoms |   1587  |  1597 |
| Tops |  4586 | 4127 |
| Intimate |  689 | 719 |
| Jackets |  413 | 466 |
| Trend |  63 | 44 |

For 𝛼 = 1/100∗max_𝑣𝑎𝑙𝑢e

| Department Distribution by Sentiment based on the ratings |   Matched  |  Did not match |
|----------|:-------------:|------:|
| Dresses |  3804 | 1567 |
| Bottoms | 2127 |  1057 |
| Tops |  5774 | 2939 |
| Intimate |  869 | 539 |
| Jackets |  552 | 327 |
| Trend |  75 | 32 |

For 𝛼 = 1/50∗max_𝑣𝑎𝑙𝑢e

| Department Distribution by Sentiment based on the ratings |   Matched  |  Did not match |
|----------|:-------------:|------:|
| Dresses |  4390 | 981 |
| Bottoms | 2503 |  681 |
| Tops |  6644 | 2069 |
| Intimate |  1011 | 397 |
| Jackets |  659 | 220 |
| Trend |  85 | 22 |

As predicted, as a increases, the values come towards the point we want. All in all, the formula seems to work, but it needs to be more refined to be precise. and we should divide the data as much as possible. because the usage of the word changes according to each class. For a more optimum result, this formula should be applied separately for each class. We are also not sure of the accuracy of the scores given. Therefore, this technique is more likely to remain as an alternative starting technique.

Data : https://www.kaggle.com/datasets/nicapotato/womens-ecommerce-clothing-reviews
