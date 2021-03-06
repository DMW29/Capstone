# Natural Language Processing
Dianne Waterson  
June 3, 2016  



## Warm-up

This document is created to satisfy Task 2: Milestone Report of the Capstone course project. The Capstone course is the last of ten courses in [John Hopkins University's Data Science Specialization](https://www.coursera.org/specializations/jhu-data-science) offered through [Coursera](http://www.coursera.org). The Capstone course project provides the opportunity to take all of the skills learned from the previous nine courses in the specialization and apply them to a real-world problem. The problem is considered real-world as there are no lectures providing significant hints to solutions, and resources are relatively limited compared to previous projects in the specialization. The solution to the problems encountered along the way, and to the project resolution, will be fully researched and created by me to simulate normal activities of a Data Scientist in industry. 

The project is developed in partnership with [SwiftKey](https://swiftkey.com/en/company/) as the company well known for their predictive text analytics. As of March 1, 2016, SwiftKey became part of the [Microsoft family](https://blog.swiftkey.com/microsoft-acquires-swiftkey/) of products. SwiftKey applications are used on Android and iOS anticipating and providing next-word choices while keyboard typing through Natural Language Processing (NLP) techniques. Microsoft's [Word Flow Technology](http://research.microsoft.com/en-us/news/features/wordflow-040414.aspx) is another example of NLP in action.

The goal of the Capstone project is to create an application that uses NLP techniques and predictive analytics, and like SwiftKey's applications, takes in a word phrase and returns next-word choices. The application will be created using RStudio's Shiny. An example of a Shiny application resides [here](https://dwaterson.shinyapps.io/NewApp/). I created this application as the final project for the ninth course in the specialization, [Developing Data Products](https://www.coursera.org/account/accomplishments/records/7LWQKYJCDK5T). This application is based on a cell phone subscription dataset containing subscription growth information for the seven regions of the world. 

The Capstone project is based on three text files containing internet blogs, news feeds, and Twitter tweets from [HCCorpora](http://www.corpora.heliohost.org/). They have been preprocessed into text files. Each text file simply contains lines of text. One line equals one blog or one news feed, for example. All forms of identification have been removed. This Milestone Report is meant to familiarize with the data contained in these files and discuss implications to NLP and the impending application. 

## Setting Expectations

Because the project goal is an application providing next-word choices based upon an input word or phrase, we must consider how many words or phrases we have in our dataset that is being used to predict the next-word choice. Our dataset consists of blogs, news, and Twitter tweets all of which use words and phrases that are easy to read and understand. The average [word count](http://answers.google.com/answers/threadview/id/709596.html) of a news story depends upon the size of the newspaper. By size is meant readership. The largest newspapers, like the New York Times, averages 1,200 words per article. Midsize papers average 800 words and the smallest newspapers, 600 words. Although people write blogs for multiple reasons, blogs are meant to attract consistent readership. This is done by minimizing the number of [characters per line](http://www.michaelleander.me/blog/tested-and-proven-formula-on-how-to-structure-blog-posts-for-maximum-readership/). A recent [study](https://blog.bufferapp.com/the-ideal-length-of-everything-online-according-to-science) found the ideal width of a blog paragraph to be 45-55 characters and the entire post at 1,600 words.Twitter tweets would seem to be inherently short and sweet. However, one [study](https://blog.bufferapp.com/the-ideal-length-of-everything-online-according-to-science) found 100 characters to be the optimum length of a tweet. 

[Readiblity](https://en.wikipedia.org/wiki/Readability) is defined by the length of text, and also speaks to the type of words used. Words, and ultimately comprehension, are elevated when they are familiar. For example, the average [newspaper is written](http://www.impact-information.com/impactinfo/literacy.htm) at the 9th grade level. The 9th grade [SAT vocabulary list](http://www.mysatwords.com/sat-vocabulary-lists/) includes words like abatement, blatant, consummate, edifice, founder, inebriation, malinger, phalanx, restitution, and vestige, to name a few. 

The corpus used for the next-word prediction algorithm and built from the three text sources; blogs, news, and tweets, then must contain enough unique words to provide accurate next-word predictions. The corpus must also contain a breadth of richness common to most people.

## Word Frequency and Zipf's Law
[Zipf's Law](https://en.wikipedia.org/wiki/Zipf%27s_law) states that given some corpus of natural language utterances, the frequency of any word is inversely proportional to its rank in the frequency table. In human langages, word frequencies have a very heavy-tailed distribution, and can be modeled reasonably well by a Zipf distribution with an s close to 1 in the formula, $$\frac{1/k^{s}}{\sum_{n=1}^{N} \frac{1}{n^{s}}}$$ where N is the number of elements, k is their rank, and s is the value characterizing the distribution. This formula is used to predict the frequency of the elements of rank K out of N elements. The [Oxford University Press](http://www.oxforddictionaries.com/us/words/how-many-words-are-there-in-the-english-language) suggests there are at least a quarter of a million distinct English words. The count excludes inflections, words from technical and regional vocabulary, and words not yet published. Then, according to Zipf's Law, the most common word will be used by an english text writer the following percentage.

```
## [1] "7.69 %"
```
Continuing with Zipf's Law, we find the number of unique words needed in a dictionary to cover 50% of the possible words used by an english text writer, is as follows.

```
## [1] "375 unique words are required in a dictionary to cover  50.02 % of all possible words used by an english text writer."
```


```
## [1] "69,000 unique words are required in a dictionary to cover  90.1 % of all possible words used by an english text writer"
```

## Application Implications

Thus, a corpus of at least 69,000 unique words should be available for a next-word choice application when > 90% language coverage is desired. The actual coverage may depend upon overall processing time. Application processing time should be invisible to the user. [For example](https://www.jstatsoft.org/article/view/v025i05), a 4,583 x 29,265 dimensioned sparse matrix processes in seven minutes on a 3.4 GHz processor and requires six megabytes of RAM. 

The files we are using for this project are comprised as follows.


|Characteristic                     |en_US.blog.txt|en_US.news.txt|en_US.twitter.txt|
|-----------------------------------|--------------|--------------|-----------------|
|File size (MB)                     |200.42        |196.28        |159.36           |
|Number of elements                 |899,288       |77,259        |2,360,148        | 
|Characters per element (avg)       |231.70        |203.0         |68.8             |
|Characters per element (max)       |40,835        |5,760         |213              |
|Words per element (avg)            |41.52         |34.22         |12.92            |
|Word length (avg)                  |4.70          |5.02          |4.48             |
|Unique words per element (avg)     |33.13         |29.77         |12.33            |         
|Unique words in file (%)           |2.96          |7.48          |17.50            |
|9th Grade SAT words in file (%)    |97.5          |89.6          |8.42             |

Together, these files require 0.5GB of processing space. This excludes other memory consuming objects. However, most systems should be capable. Even cell phones come with 16GB of processing power. Processing time is determined by the algorithm. All will be done to maximize algorithm speed to enable a comprehensive corpus.

The characteristics of the tweets in the Twitter file fall a little short of expectations. Whereas the optimum tweet is 100 characters, our file contains an average of 68 characters per tweet. The expected length of a blog post is 1,600 words. The average number of words in our file for blogs is 41. There exists a blog of 40,835 characters in our file containing blogs. This should be inspected further because the 75th quantile is at 331 characters making 40,835 an outlying data point. The same is true of news texts in our file. The number of words is below expectations with an outlier at 5,760 characters as the 75th quantile is at 270.  

Considering readibility, we see that the file containing tweets provides a high level of word uniqueness. Although tweets provide a higher level of word uniqueness, the number of words in the average tweet is small at 12. We also notice Tweet authors on a whole do not use much 9th grade level vocabulary. Blogs and news feeds do not excel at word uniqueness, but do provide language richness. This is apparent from the high percentage of 9th grade SAT vocabulary words used. Because blogs and news feeds contain a large average number of words, they can provide more next-word choices for the predictive algorithm compared with tweets. 

The number of unique words could be enhanced in the corpus with supplemenation from an authoritative dictionary. Although, this will not help with grammer enhancement. An authoritative dictionary can also be used as comparison against the corpus created for the application to remove foreign words. For example, an authoritative dictionary will contain foreign words formally adapted into the language. An example of this is the word kindergarten. Kindergarten is a german word formally adapted into the English language and found in authoritative dictionaries. On the other hand, an authoritative dictionary of English will not contain Chinese characters. 

Consideration toward reducing the number of words can be achieved through substitution with synonyms. For all words meaning the same, one word could be used. This reduces corpus size while maintaining next-word choice opportunities. Although, will have the effect of reducing language richness of the corpus.

## Unigrams, Bigrams, and Trigrams

Following is a wordcloud of unigrams and distributions of 2- and 3-gram phrases. These are calculated from combining 100 samples from each of the three text files into one corpus. Numbers, stopwords, whitespace, punctuation, and derogatory items have been removed. Although, it appears there remains extraneous characters yet to be cleaned.

![](NLP_files/figure-html/ngrams-1.png)![](NLP_files/figure-html/ngrams-2.png)![](NLP_files/figure-html/ngrams-3.png)


