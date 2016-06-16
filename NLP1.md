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

Following is a wordcloud of unigrams and distributions of 2-, 3-, and 4-gram phrases. These are calculated from combining 100 samples from each of the three text files into one corpus. Numbers, stopwords, whitespace, punctuation, and derogatory items have been removed. 

```r
if (!require(tm)) {
     install.packages("tm", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(tm)
}
```

```
## Loading required package: tm
```

```
## Loading required package: NLP
```

```r
if (!require(stringr)) {
     install.packages("stringr", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(stringr)
}
```

```
## Loading required package: stringr
```

```r
if (!require(quanteda)) {
     install.packages("quanteda", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(quanteda)
}
```

```
## Loading required package: quanteda
```

```
## quanteda version 0.9.6.9
```

```
## 
## Attaching package: 'quanteda'
```

```
## The following objects are masked from 'package:tm':
## 
##     as.DocumentTermMatrix, stopwords
```

```
## The following object is masked from 'package:NLP':
## 
##     ngrams
```

```
## The following object is masked from 'package:base':
## 
##     sample
```

```r
if (!require(wordcloud)) {
     install.packages("wordCloud", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(wordcloud)
}
```

```
## Loading required package: wordcloud
```

```
## Loading required package: RColorBrewer
```

```r
if (!require(RWeka)) {
     install.packages("RWeka", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(RWeka)
}
```

```
## Loading required package: RWeka
```

```r
if (!require(ggplot2)) {
     install.packages("ggplot2", repos = "http://cran.r-project.org", dependencies = TRUE)
     require(ggplot2)
     }
```

```
## Loading required package: ggplot2
```

```
## 
## Attaching package: 'ggplot2'
```

```
## The following object is masked from 'package:NLP':
## 
##     annotate
```

```r
## Set working directory
setwd("~/")

## Sample the text documents to speed learning
smaller <- function(x) {
     x <- sample(x, length(x)*0.0009999)
}

set.seed(455)
textFiles <- c("en_US.twitter.txt", "en_US.blogs.txt", "en_US.news.txt")
blogSamp <- smaller(readLines(file(textFiles[1],open = "r"), 
                              n=-1, skipNul = TRUE, 
                              encoding = "UTF-8"))
newsSamp <- smaller(readLines(file(textFiles[2],open = "r"), 
                              n=-1, skipNul = TRUE, 
                              encoding = "UTF-8"))
twitSamp <- smaller(readLines(file(textFiles[3],open = "r"), 
                              n=-1, skipNul = TRUE, 
                              encoding = "UTF-8"))
```

```
## Warning in readLines(file(textFiles[3], open = "r"), n = -1, skipNul =
## TRUE, : incomplete final line found on 'en_US.news.txt'
```

```r
## Clean text samples after joining the three text source file samples
## The order in which the text are cleaned does matter
badWordsList <- readLines("Bad Word List.csv", n=-1, skipNul = TRUE)

groupText <- c(blogSamp, newsSamp, twitSamp)

groupText <- iconv(groupText, "latin1", "ASCII", sub="")
groupText <- tolower(groupText)
groupText <- removeNumbers(groupText)
groupText <- removePunctuation(groupText, preserve_intra_word_dashes = TRUE)
groupText <- gsub("http[[:alnum:]]*", "", groupText)
groupText <- removeWords(groupText, badWordsList)
groupText <- stripWhitespace(groupText)
groupText <- str_trim(groupText, side = c("both"))
groupText <- gsub("\u0092", "'", groupText)
groupText <- gsub("\u0093|\u0094", "", groupText)

groupCorp <- VCorpus(VectorSource(groupText))
```

```
## Warning: closing unused connection 5 (en_US.news.txt)
```

```r
## create word cloud
## Reference: https://sites.google.com/site/miningtwitter/questions/talking-about/wordclouds/wordcloud1
tdm = TermDocumentMatrix(groupCorp)
```

```
## Warning: closing unused connection 6 (en_US.blogs.txt)
```

```r
# define tdm as matrix
m = as.matrix(tdm)
# get word counts in decreasing order
word_freqs = sort(rowSums(m), decreasing=TRUE) 
# create a data frame with words and their frequencies
dm = data.frame(word=names(word_freqs), freq=word_freqs)
# plot wordcloud
wordcloud(dm$word, dm$freq, random.order=FALSE, colors=brewer.pal(8, "Dark2"))
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : process could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : research could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : results could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : somewhere could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : straight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : themselves could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tweeting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : waiting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : weather could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whatever could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wondering could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : youve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agreed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : america could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : amount could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : answer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beginning could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : breakfast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : building could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : busy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : butter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : calling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : college could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : contact could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : continue could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : control could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dead could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : death could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : decided could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : degrees could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fans could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : george could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : given could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : happen could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : however could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : husband could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : image could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inches could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : involved could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : issue could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : issues could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : itself could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jesus could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : july could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kitchen could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : large could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : learned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lovely could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lucky could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : matter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : moon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : omg could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : others could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : passed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : peoples could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : piece could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : problems could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : question could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ran could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rights could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seeing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : serious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seriously could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : service could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solid could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stories could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : success could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : teacher could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : totally could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : trade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : travel could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : truly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : trust could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : war could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : weve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wine could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : works could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yall could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ability could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : according could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : account could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : added could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anyway could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : artist could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : basic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : behind could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : board could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : boys could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bro could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : calls could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : camera could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cannot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cars could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : characters could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chicago could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chicken could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : click could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : competition could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : complete could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : connection could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : content could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cost could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : count could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : county could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : data could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : definitely could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dress could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drop could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : due could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : entire could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : excuse could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fashion could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : feels could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : finished could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fish could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : games could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gives could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hanging could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : happening could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : health could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : held could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : helping could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hop could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hospital could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hour could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : huge could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : interview could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : james could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : killing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : known could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : land could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : looked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mad could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : major could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mark could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : men could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : menu could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : missed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : missing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : monday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : month could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mothers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : moved could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nearly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : note could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : novel could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : oil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : opening could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : original could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : outside could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : paint could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : perhaps could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : personal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : photos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : planning could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : played could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : potential could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : present could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : president could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : products could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : published could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : putting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : questions could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : regional could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : response could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : review could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : round could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sea could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : secret could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : send could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : services could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sharing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sign could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : simply could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sitting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smile could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : song could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : south could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : starting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : step could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : steps could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stopped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : student could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suggestions could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : supposed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : table could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : takes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tape could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tea could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : teams could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : unfortunately could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : various could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : walk could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wants could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wedding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : winner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : worry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : worth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : acting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : action could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : actor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : airlines could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : attention could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : background could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blessed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blogs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bottom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : british could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brown could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cheek could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : childhood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : choose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : classic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : coach could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : collection could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : comments could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cook could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : copy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : creative could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : customer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : david could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : district could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : door could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dreams could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drinking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dude could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : eating could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : editor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : email could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ends could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : enjoying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : exciting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : extra could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : farmers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : figure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : filled could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : floor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : folks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : former could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : giant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : government could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : green could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : groups could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hall could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : handle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hands could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : happens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hockey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hold could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hopefully could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hurts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : images could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : included could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : info could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : interested could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : international could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : january could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jason could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knowing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : laws could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : letters could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : links could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : loving could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : manner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : march could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : member could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : message could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : midnight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : milk could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : minute could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mode could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mostly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : notice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : odbd could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : official could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : opportunity could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : organized could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pass could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pink could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : posted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : previous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : price could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : projects could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : quick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : race could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : realized could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recently could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rules could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seemed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : selling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : silly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : society could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stomach could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : strength could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tim could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : train could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : treat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : understand could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : update could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vacation could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : visiting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : warm could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : washington could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : watched could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : web could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : website could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : within could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wrote could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yard could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agree could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : album could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : allowed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : alone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : app could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : apparently could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : arts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : association could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : avengers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : average could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bar could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : below could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : besides could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : born could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : boyfriend could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : broke could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cafe could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : campbells could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : career could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cats could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : changes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chapter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : choice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : christ could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : christian could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : climate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : color could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : committee could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : companies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : computer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : conference could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cooking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : corner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : corporate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cream could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : credit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cried could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : current could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : currently could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dangerous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dec could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : decision could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : delicious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : diana could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : diet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : disease could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dream could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dressed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : driving could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : east could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ended could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : eventually could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : everyday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : exactly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : excellent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : except could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : exchange could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : expected could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : express could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : featuring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : federal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : feed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : florida could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : flower could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : flowers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : followed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : garden could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : glass could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hahaha could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : happiness could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hats could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : headed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : heading could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hearts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : holding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : icing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : india could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : industry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inspired could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : interest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ireland could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : island could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jacket could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jersey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : join could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : liberal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : limited could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : losing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lower could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lupus could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : marketing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : members could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : memories could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : memory could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mentioned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : michael could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : miles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mission could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : moms could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : movement could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : movies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : negative could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nights could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : non could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : normal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : north could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nyc could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : offer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : organization could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pack could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : painful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : painted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : particular could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : places could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : player could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : points could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : political could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : politics could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pool could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : powerful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pull could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pushing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : random could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : range could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rangers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : readers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reality could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : receive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recognize could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : regular could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : relationship could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : remind could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : request could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : risk could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sample could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : scene could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : senior could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sets could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shall could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : showed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : showing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : significant could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : situation could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smaller could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solution could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sort could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : space could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : specific could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : star could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : states could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stores could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : strange could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : studio could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : supporting could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : symbol could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : systems could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : taste could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : text could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tips could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tools could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : topic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : training could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : truck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tuesday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : turned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : twins could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : uncle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vegas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : views could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vote could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : walking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wilson could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : windows could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : zone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : absolutely could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : activities could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : activity could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : admit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : afternoon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agreement could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : annual could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : apart could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : application could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : arrested could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : assume could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : authors could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : avoid could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : awkward could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : baptized could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : basis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beach could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : became could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beef could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : begins could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bible could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : biggest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blame could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blogger could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bond could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : booth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bottle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bread could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bringing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : buildings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : california could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : campus could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : caught could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cheers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chris could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : circling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : citizens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : civil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clients could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : closed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : complex could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : consider could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : contest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : council could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : coverage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crisis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : culture could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dallas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dancing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deep could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : describe could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : died could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : difficult could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : direction could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : directly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dirty could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : discovered could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : discuss could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : discussing could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : doubt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : draw could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : earlier could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : effective could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : effort could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : english could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : entertainment could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : entry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : episode could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : examples could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : failure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fantastic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : faster could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : feature could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : feelings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : field could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : finish could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : flag could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : focus could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : football could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : forgot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : freaking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fully could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : further could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : general could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : global could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gods could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goodness could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : google could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grew could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : guilty could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hang could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : heads could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hearing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : heaven could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : helps could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hilarious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hire could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : history could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hitting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : holiday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : holy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : homemade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : homie could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : horrible could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ilsa could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : immediately could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : impact could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inc could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : incident could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : includes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : informed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : insane could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : internet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jewish could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jobs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : joy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : june could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knowledge could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : language could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : laughing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leading could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : league could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leaving could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : letter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : letting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : library could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lightly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : liked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : literally could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : loss could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : loves could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mail could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : management could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : marathon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : marion could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : married could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mccammon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : merry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mobile could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : moral could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mouth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mum could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : museum could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : named could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nap could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : natural could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nature could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : neighbors could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nervous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : niggas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : operates could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : opinion could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : owned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : owner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pages could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : parent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : participate could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : partner could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : parts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pepper could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : physical could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pieces could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pizza could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : planned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : posts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pouch could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ppl could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : practical could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pray could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prayers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pregnant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pride could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : princess could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : produce could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : proud could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : publishing could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : puts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : quickly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reach could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reasons could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recipes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recommend could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : referred could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : relationships could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : relief could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : religious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rep could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : result could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reviews could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reward could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : road could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rocks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ryan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : safety could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : salad could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sale could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sam could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : search could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : serve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sexy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shirt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shoes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shop could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shoulder could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sigh could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : signed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : silent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : similar could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sisters could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sites could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sketch could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : skywest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : slightly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : slow could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : slowly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smith could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smoking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : somebody could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : soup could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spoke could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sports could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : square could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : staff could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stand could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stressed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : struggle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stuck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : subject could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suddenly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sunny could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tears could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : third could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tho could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thousands could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thumb could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ticket could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tiny could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : track could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : turning could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tweeted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : type could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : unless could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ups could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : usually could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : version could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wall could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ways could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : weight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wife could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : willing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : winning could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : womens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : worse could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aaron could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : absence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : accept could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : actions could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : actual could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : address could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : adventures could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : advertising could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agency could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agents could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aircraft could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : alive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : allow could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : alot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : americans could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : analysis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : andrew could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : angel could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : animal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ann could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : announced could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : answers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anymore could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : apartment could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aspects could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : attack could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : avenue could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : awards could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : awareness could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : baking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : balance could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ball could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : baltimore could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : banks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : base could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : baseball could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bbq could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : begin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : behavior could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : benefit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : benefits could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bikes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blessings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blogging could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bobby could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : boil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : boring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : breath could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brewers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brewing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : broken could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bug could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bunny could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : burn could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bush could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : canada could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : candidate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : canon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cap could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cards could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : careful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carlos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cash could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : celebrate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : celebration could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : changed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : checked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cherubim could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chili could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chill could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cinnamon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : circumstances could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : claim could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : classes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clothes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clothing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cmon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : codes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : comfort could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : comfortable could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : complain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : complicated could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : concerned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : concert could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : conflict could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : confusion could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : constantly could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : contain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : continued could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : contract could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : contractual could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : controversy could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cookie could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : costs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : covered could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crack could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crawford could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : creations could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crowd could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crown could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cuz could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : daddy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dealing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : decade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : december could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : decide could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : decisions could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : delicate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : delta could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : demons could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deserve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : designs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : despite could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : detail could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : details could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : development could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : difference could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : digital could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : discount could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : display could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : domestic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : donate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : downs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : downtown could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drama could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dried could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dropped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drove could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : duke could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : easter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : edge could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : editing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : egg could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : employment could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : enemy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : explained could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fabulous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : falling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : farm could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fascinating could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : favor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : favourite could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : featured could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : festival could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fill could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : filming could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : finding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fingers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : flying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : follows could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : foods could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : forced could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : forgotten could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : forum could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : foundation could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fresh could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : frugivorous could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gallon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : georgia could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gettin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : girlfriend could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goals could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : golden could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grandmother could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grass could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gross could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ground could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grown could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : happier could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hated could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : helped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : historical could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hollywood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hook could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hosting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : household could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : houston could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hurdle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ima could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : immediate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : immigration could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : importance could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : improve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : include could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : indeed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inspiring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : instant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : intellectual could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : intelligence could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : iphone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : issued could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : item could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : items could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jack could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jaglom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : japan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jared could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : joe could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : joke could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : journey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jump could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : justice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kidding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kindle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kiss could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knee could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knicks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knife could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ktia could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : labour could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lamb could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : landed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lately could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : latest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : laugh could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lawrence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leaders could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : leadership could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leg could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lessons could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lights could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : likes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : listing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : london could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : louis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : managers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : marked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : match could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : material could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : math could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : matt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : matters could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meets could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : melt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : memphis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mentor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mess could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : metro could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mile could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : military could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mini could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mistake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : modern could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : moments could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : monkey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : monthly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mystery could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nation could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : niece could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : none could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : normally could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : noticed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : obama could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ofelia could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : offering could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : offers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : officially could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : officials could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : older could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : olive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : onto could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : operational could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ourselves could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : outcome could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : painting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pale could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : parties could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : passenger could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : path could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : patricks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pencil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : per could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : perfectly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : performing could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : permanent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : personally could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : phil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pitcher could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : players could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pocket could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : police could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : poor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : popping could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : positive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prepare could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pressure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pretend could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prize could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : profile could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : promises could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : provide could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : publication could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : punch could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pyramid could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : queen could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : radio could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recommendations could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : related could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : release could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : remaining could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : repeat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : report could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : returned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : riding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rising could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rob could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : role could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : route could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : routine could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : royal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : runs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : salt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : santa could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sauce could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : scared could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : screaming could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : security could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seem could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sees could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : september could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : session could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shades could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shouldnt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shout could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sides could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : silence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : silver could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sixth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : skin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sleeping could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smell could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smoke could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smooth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : snow could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : son could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sorts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : source could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : split could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spray could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stamp could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : standing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : steve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stock could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stopping could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stops could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : strawberries could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stronger could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : struggling could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stupid could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : submit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : substitute could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : successful could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suggested could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suppose could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : surely could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : swear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : talented could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : taylor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : technical could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : technically could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : techniques could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : technology could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : telling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : temple could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : testimony could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thai could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thankful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thanx could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : theme could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : theory could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thomas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : throughout could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : throw could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : title could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : treated could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tribute could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : turns could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : types could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : unique could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : unknown could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : unusual could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : uses could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vancouver could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : vegetables could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : veggies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : videos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vietnam could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vigilant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : visited could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : visits could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wanting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : warmth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wearing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : weed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : western could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : workout could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : workshop could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : written could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yay could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : year-old could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yellow could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : yesterdays could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : youd could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : youtube could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : abt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : accepted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : accounts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : accused could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : achieve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : acts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : adapt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : adults could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : affairs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : africa could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aged could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : agenda could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ages could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : agreements could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : airline could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aka could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : allegedly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : allowing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : alumni could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : amongst could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : analyst could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : andor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : angeles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anime could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anne could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anniversary could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : annoyed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : answered could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anxious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anybody could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : anywhere could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : appear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : approach could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : areas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : arena could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ark could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : arms could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : array could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : arrived could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : articles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : asleep could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : assist could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : assumed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : attacks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : attending could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : attracted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : audience could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : august could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : authority could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : avoided could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : award could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : awful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : aww could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : backwards could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : balls could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : balsamic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bank could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : barely could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bases could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : basics could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bay could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bear could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bearing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bears could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beating could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beauty could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : becoming could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : behaviors could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : belief could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bell could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bellilin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : belly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : belong could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : beyond could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bike could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bitching could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : blowing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : booked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bother could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : breaking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : breeze could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bright could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : broncos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : broom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : brothers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bruce could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : buddy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : budget could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : build could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : built could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bullying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : burnt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : buzzard could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : bye could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cancer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : candidates could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : candy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : canvas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : capable could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carefully could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carried could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carrot could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : carry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cases could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : catching could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : centre could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cercopithecus could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cereal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ceremony could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : certain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : certainly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chair could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : challenges could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : changing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : charge could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : checking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cheese could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : chick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : childrens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : classmates could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : classroom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cleaned could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clearly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : clever could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : closer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : closest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : closet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : code could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : collaboration could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : colorful could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : colors could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : combination could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : comedy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : companys could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : comparing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : compelling could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : compliments could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : confidence could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : conjure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : conservative could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : considered could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : construction could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : continues could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : convention could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cookies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : correct could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : corruption could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : countries could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : counts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : courage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : courts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cousin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : covers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : craft could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : creating could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : creepy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crew could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crossed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : crystal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cups could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : curious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : cutting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : daisy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dang could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : daughter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deadline could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deadly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deals could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : debate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : debt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : decorating could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : def could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : defined could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : delayed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : demand could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : denver could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : deny could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : destiny could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : devastating could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : develop could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dialogue could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : diego could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : differences could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : digest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : direct could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dirt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : discover could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dish could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : distinctive could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : doctor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : double could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dove could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dragon could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drawings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drawn could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drew could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : drug could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dunham could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : dyanne could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : economic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : edition could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : effect could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : effectively could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : efforts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : eight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : elements could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : emotional could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : employees could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : encourage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ending could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : enemies could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : engaged could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : enjoyable could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : environment could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : environmental could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : episodes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : era could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : eternal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : everywhere could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : evidence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : evil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : exclusive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : executive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : exercise could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : expectations could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : expensive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : experienced could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : experiences could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : expert could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : explain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : export could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : expressed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : extend could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : extremely could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : faces could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : factor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : factory could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : facts could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fair could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : famous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fathers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fault could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fee could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fellow could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fifteen could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : films could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : finances could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : financial could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : finds could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fired could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : firm could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fixed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : flood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : focused could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fourth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : francis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : frank could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : freak could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : freakin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : freaky could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : freedom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : french could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fridge could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : friendship could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : fruits could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : funding could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gandhi could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gates could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : generally could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : generous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : german could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : glory could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gold could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : golf could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goodbye could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goodnight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : goods could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gorgeous could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gosh could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gov could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grabbed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grace could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grand could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : greatest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : greek could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grenade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : grocery could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : growing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : guan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : guest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : guests could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : guitarist could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gun could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gunna could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : gym could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hadnt could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hail could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : halloween could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : handy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : harder could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hates could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hawkeye could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : heavy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : heights could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : herbs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hero could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hidden could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : highlights could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hiring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : honor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hood could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hoping could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hosted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hotel could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hug could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : humor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hundreds could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : hunger could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : idol could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : inch could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : increase could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : incredible could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : individual could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : individuals could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : influence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : infrastructure could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : injury could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : innovation could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : insight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : instance could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : interviews could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ipa could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ish could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : islam could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ito could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jackson could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jam could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jams could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : janet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jazz could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jeanne could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jesse could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jimmy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jordan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : juice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jumped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : jury could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : justify could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : justin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kansas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kaye could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : keeps could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : kiddos could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : killed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : king could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : knock could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lace could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lakers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lambic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : landing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lands could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : larne could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lasted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lawn could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : layer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : layers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lead could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leader could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : leaves could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : legs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : levels could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lifetime could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lineup could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lived could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : location could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : locations could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : lookin could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : maintaining could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : manage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : managed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : margarine could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : markets could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : marriage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mask could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : massive could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : master could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : matching could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meals could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : measure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : mechanical could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : medical could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : medium could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meetings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : megan could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : metal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : method could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mexico could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meyer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : meyers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : michaels could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : michelle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mike could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : miley could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : min could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mines could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : minor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mistakes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : model could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : momma could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mortgage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : mulcahy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : multiple could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : murray could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : musician could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : myth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : names could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : neck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : neighbor could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : neills could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : netflix could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : networking could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : newspaper could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nicely could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : nine could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : norm could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : noted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : notes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : november could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : obstacles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : obtain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : obvious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : obviously could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : occupy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : onions could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : opens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : opposite could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : opposition could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : options could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ordered could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : organizations could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : organize could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : organizing could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : orioles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : overall could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : owners could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pace could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : package could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : parade could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : paris could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : parking could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : passes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : patterns could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : paycheck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pedal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : percent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : period could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : perspective could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : phones could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : photography could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : piano could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pics could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : placed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : plain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : plant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : plants could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : plastic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : plays could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : poems could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : politicians could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : popular could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : portion could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : possibilities could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : posting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pounds could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pour could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : powder could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : practice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prawns could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : praying could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : precious could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : predictable could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : premium could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : preparation could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prepared could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : presence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : primary could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prince could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : print could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : produced could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : product could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : programs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : promise could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : pronounced could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : prosecutors could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : protect could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : protest could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pub could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : publishers could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : puppy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : purchase could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : purchased could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : pure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : purple could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : quote could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : quotes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : racial could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : racing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : radiation could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : raining could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rampant could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rapid could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rates could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reaches could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reader could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reads could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recipient could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recognizing could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : recommendation could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : record could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : refuse could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : refused could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rehearsal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : relate could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : remain could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : remote could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : remove could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : removed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : resources could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : respect could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : restaurant could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : restore could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : returns could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reveals could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : reviewing could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rings could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rita could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : romney could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rotary could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rough could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rounded could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : row could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ruffle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rush could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : rushes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sacrifice could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sales could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : satisfaction could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : science could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : scientist could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : scott could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : scrap could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : searching could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seasonal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seasons could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seconds could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : section could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : select could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : setting could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : settle could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : seven could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sex could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sexual could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shake could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shared could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sheet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shells could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ship could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shots could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shouldve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shower could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shrubs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : shut could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sidebar could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : silk could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : singer could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sins could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sky could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : skype could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smart could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : smh could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : snapped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : soda could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : softball could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solitude could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solo could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solutions could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : solve could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sometime could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : songs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : soo could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spam could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : speaks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : speeches could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : speed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spending could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spirit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : spot-nosed could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : spread could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : springs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stage could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stairs could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : starbucks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : statement could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : station could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : steaks could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : steal could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stirring could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stone could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : stretches could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : struck could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : structure could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : study could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : suburban could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : summit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : surprise could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : surprised could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : survived could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : sweat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : swimsuit could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : symbols could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : synopsis could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : talked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tank could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : targets could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : task could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tasty could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tax could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : taxes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : teachers could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : teens could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : template could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : terms could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : terpil could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : terrible could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : terrified could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : terry could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : texas could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : threw could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thru could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thud could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : thx could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tiger could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tiles could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : timeline could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : toddler could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ton could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : topped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tops could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : toronto could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tough could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : toward could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : towards could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : towel could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : traditional could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : traffic could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tragedy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : traveled could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : treating could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : trick could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : troy could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : trusted could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : tune could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : turkey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : twilight could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : typical could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : ugly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors
## = brewer.pal(8, : underneath could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : understanding could not be fit on page. It will not be
## plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : upload could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : upper could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : upset could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : users could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : usual could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : utterly could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : value could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : variety could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vast could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : venison could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : venture could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : verses could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : victims could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : violence could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vip could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : vision could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : voices could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : waited could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wales could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : walked could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : walls could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wash could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : waves could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : websites could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wed could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wednesday could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wen could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : werent could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wet could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wheat could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whenever could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wheres could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whilst could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whipped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whiskey could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whitney could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : whom could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wide could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wins could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : winter could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wrapped could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : writes could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : wth could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yards could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yelling could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yep could not be fit on page. It will not be plotted.
```

```
## Warning in wordcloud(dm$word, dm$freq, random.order = FALSE, colors =
## brewer.pal(8, : yoga could not be fit on page. It will not be plotted.
```

![](NLP1_files/figure-html/ngrams-1.png)

```r
##f <- findFreqTerms(tdm, 0, 31)
##f[1:20]

## Look for bigrams
BigramTokenizer <- function(x) NGramTokenizer(x, Weka_control(min = 2, max = 2))
tdmTok <- TermDocumentMatrix(groupCorp, control = list(tokenize = BigramTokenizer))
inspect(tdmTok[26010:26070,20:40])
```

```
## <<TermDocumentMatrix (terms: 61, documents: 21)>>
## Non-/sparse entries: 1/1280
## Sparsity           : 100%
## Maximal term length: 18
## Weighting          : term frequency (tf)
## 
##                     Docs
## Terms                20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37
##   of speaking         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of spillage         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of spring           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of staff            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of stairs           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of staple           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of state            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of steam            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of stickers         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of stony            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of strangers        0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of strength         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of students         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of stuff            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of style            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of sub-plots        0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of such             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of sugar            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of summer           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of super-formalist  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of supernatural     0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of supposed         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of sweet            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of swimsuits        0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of swiss            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of switzerland      0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of tables           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of taking           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of talent           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of talk             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of tape             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of tea              0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of ted              0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of terrible         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of terrorism        0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of textures         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of th               0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of thailand         0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of that             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of the              0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1  0
##   of their            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of them             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of themselves       0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of themso           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of there            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of these            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of things           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of this             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of those            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of thousands        0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of three            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of thrones          0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of thumb            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of tim              0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of time             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of timers           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of to               0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of today            0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of tone             0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of too              0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##   of travel           0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
##                     Docs
## Terms                38 39 40
##   of speaking         0  0  0
##   of spillage         0  0  0
##   of spring           0  0  0
##   of staff            0  0  0
##   of stairs           0  0  0
##   of staple           0  0  0
##   of state            0  0  0
##   of steam            0  0  0
##   of stickers         0  0  0
##   of stony            0  0  0
##   of strangers        0  0  0
##   of strength         0  0  0
##   of students         0  0  0
##   of stuff            0  0  0
##   of style            0  0  0
##   of sub-plots        0  0  0
##   of such             0  0  0
##   of sugar            0  0  0
##   of summer           0  0  0
##   of super-formalist  0  0  0
##   of supernatural     0  0  0
##   of supposed         0  0  0
##   of sweet            0  0  0
##   of swimsuits        0  0  0
##   of swiss            0  0  0
##   of switzerland      0  0  0
##   of tables           0  0  0
##   of taking           0  0  0
##   of talent           0  0  0
##   of talk             0  0  0
##   of tape             0  0  0
##   of tea              0  0  0
##   of ted              0  0  0
##   of terrible         0  0  0
##   of terrorism        0  0  0
##   of textures         0  0  0
##   of th               0  0  0
##   of thailand         0  0  0
##   of that             0  0  0
##   of the              0  0  0
##   of their            0  0  0
##   of them             0  0  0
##   of themselves       0  0  0
##   of themso           0  0  0
##   of there            0  0  0
##   of these            0  0  0
##   of things           0  0  0
##   of this             0  0  0
##   of those            0  0  0
##   of thousands        0  0  0
##   of three            0  0  0
##   of thrones          0  0  0
##   of thumb            0  0  0
##   of tim              0  0  0
##   of time             0  0  0
##   of timers           0  0  0
##   of to               0  0  0
##   of today            0  0  0
##   of tone             0  0  0
##   of too              0  0  0
##   of travel           0  0  0
```

```r
mBi <- as.matrix(tdmTok)
word_freqs_bi <- sort(rowSums(mBi), decreasing = TRUE)
dmBi <- data.frame(word = names(word_freqs_bi), freq = word_freqs_bi)
dmBi20 <- dmBi[1:20,]
## Plot bi-grams
gBi <- ggplot(dmBi20, aes(reorder(word, -freq), freq))
gBi <- gBi + geom_bar(stat = "identity", fill = I("grey50"))
gBi <- gBi + labs(x = "Bigram", y = "Frequency")
gBi <- gBi + theme(axis.text.x = element_text(angle = 90, size = 12, hjust = 1))
gBi
```

![](NLP1_files/figure-html/ngrams-2.png)

```r
## Look for tri-grams
TrigramTokenizer <- function(x) NGramTokenizer(x, Weka_control(min = 3, max = 3))
tdmTok3 <- TermDocumentMatrix(groupCorp, control = list(tokenize = TrigramTokenizer))
inspect(tdmTok3[225:255,1:20])
```

```
## <<TermDocumentMatrix (terms: 31, documents: 20)>>
## Non-/sparse entries: 0/620
## Sparsity           : 100%
## Maximal term length: 23
## Weighting          : term frequency (tf)
## 
##                          Docs
## Terms                     1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
##   a couple what           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a court martial         0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a coward hides          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crab dip              0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a craft store           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a creative way          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a creepy owner          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crib size             0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crime or              0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a criminal lawyer       0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a criminal or           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crisis against        0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crisis that           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a critique of           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a critiquing service    0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crowd at              0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a crucial liberal       0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a cup of                0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a cute guy              0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a cute little           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a cyber slave           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a cybersecurity corpsof 0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a daily basis           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a damask design         0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a dan lepard            0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a dan sir               0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a dang ankle            0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a danger to             0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a dangerousness to      0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a daughter like         0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##   a day cleanse           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0  0
##                          Docs
## Terms                     20
##   a couple what            0
##   a court martial          0
##   a coward hides           0
##   a crab dip               0
##   a craft store            0
##   a creative way           0
##   a creepy owner           0
##   a crib size              0
##   a crime or               0
##   a criminal lawyer        0
##   a criminal or            0
##   a crisis against         0
##   a crisis that            0
##   a critique of            0
##   a critiquing service     0
##   a crowd at               0
##   a crucial liberal        0
##   a cup of                 0
##   a cute guy               0
##   a cute little            0
##   a cyber slave            0
##   a cybersecurity corpsof  0
##   a daily basis            0
##   a damask design          0
##   a dan lepard             0
##   a dan sir                0
##   a dang ankle             0
##   a danger to              0
##   a dangerousness to       0
##   a daughter like          0
##   a day cleanse            0
```

```r
mTri <- as.matrix(tdmTok3)
word_freqs_tri <- sort(rowSums(mTri), decreasing = TRUE)
dmTri <- data.frame(word = names(word_freqs_tri), freq = word_freqs_tri)
dmTri20 <- dmTri[1:20,]
## Plot bigrams
gBi <- ggplot(dmTri20, aes(reorder(word, -freq), freq))
gBi <- gBi + geom_bar(stat = "identity", fill = I("grey50"))
gBi <- gBi + labs(x = "Trigram", y = "Frequency")
gBi <- gBi + theme(axis.text.x = element_text(angle = 90, size = 12, hjust = 1))
gBi
```

![](NLP1_files/figure-html/ngrams-3.png)

```r
## Look for quad-grams
TrigramTokenizer <- function(x) NGramTokenizer(x, Weka_control(min = 4, max = 4))
tdmTok4 <- TermDocumentMatrix(groupCorp, control = list(tokenize = TrigramTokenizer))
inspect(tdmTok4[225:255,1:20])
```

```
## <<TermDocumentMatrix (terms: 31, documents: 20)>>
## Non-/sparse entries: 0/620
## Sparsity           : 100%
## Maximal term length: 26
## Weighting          : term frequency (tf)
## 
##                             Docs
## Terms                        1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18
##   a couple of dogs           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of hours          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of links          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of miles          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of new            0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of seats          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of sub-plots      0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of the            0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of vaguely        0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of weeks          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple of years          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple other friends     0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple other look-at-me  0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple really pronounced 0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple times and         0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a couple what is           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a court martial powell     0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a coward hides and         0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crab dip it              0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a craft store to           0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a creative way suggestions 0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a creepy owner haha        0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crib size shirt          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crime or even            0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a criminal lawyer now      0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a criminal or civil        0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crisis against which     0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crisis that has          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a critique of his          0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a critiquing service for   0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##   a crowd at baltimore       0 0 0 0 0 0 0 0 0  0  0  0  0  0  0  0  0  0
##                             Docs
## Terms                        19 20
##   a couple of dogs            0  0
##   a couple of hours           0  0
##   a couple of links           0  0
##   a couple of miles           0  0
##   a couple of new             0  0
##   a couple of seats           0  0
##   a couple of sub-plots       0  0
##   a couple of the             0  0
##   a couple of vaguely         0  0
##   a couple of weeks           0  0
##   a couple of years           0  0
##   a couple other friends      0  0
##   a couple other look-at-me   0  0
##   a couple really pronounced  0  0
##   a couple times and          0  0
##   a couple what is            0  0
##   a court martial powell      0  0
##   a coward hides and          0  0
##   a crab dip it               0  0
##   a craft store to            0  0
##   a creative way suggestions  0  0
##   a creepy owner haha         0  0
##   a crib size shirt           0  0
##   a crime or even             0  0
##   a criminal lawyer now       0  0
##   a criminal or civil         0  0
##   a crisis against which      0  0
##   a crisis that has           0  0
##   a critique of his           0  0
##   a critiquing service for    0  0
##   a crowd at baltimore        0  0
```

```r
mTri <- as.matrix(tdmTok4)
word_freqs_tri <- sort(rowSums(mTri), decreasing = TRUE)
dmTri <- data.frame(word = names(word_freqs_tri), freq = word_freqs_tri)
dmTri20 <- dmTri[1:20,]
## Plot bigrams
gBi <- ggplot(dmTri20, aes(reorder(word, -freq), freq))
gBi <- gBi + geom_bar(stat = "identity", fill = I("grey50"))
gBi <- gBi + labs(x = "Quadgram", y = "Frequency")
gBi <- gBi + theme(axis.text.x = element_text(angle = 90, size = 12, hjust = 1))
gBi
```

![](NLP1_files/figure-html/ngrams-4.png)

```r
## Look for quint-grams
TrigramTokenizer <- function(x) NGramTokenizer(x, Weka_control(min = 5, max = 5))
tdmTok5 <- TermDocumentMatrix(groupCorp, control = list(tokenize = TrigramTokenizer))
inspect(tdmTok5[225:255,1:20])
```

```
## <<TermDocumentMatrix (terms: 31, documents: 20)>>
## Non-/sparse entries: 0/620
## Sparsity           : 100%
## Maximal term length: 38
## Weighting          : term frequency (tf)
## 
##                                         Docs
## Terms                                    1 2 3 4 5 6 7 8 9 10 11 12 13 14
##   a couple other look-at-me blogs        0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a couple really pronounced rushes      0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a couple times and get                 0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a couple what is inextricably          0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a court martial powell sided           0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a coward hides and inflicts            0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crab dip it was                      0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a craft store to see                   0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a creative way suggestions included    0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crib size shirt using                0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crime or even a                      0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a criminal lawyer now defamation       0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a criminal or civil matter             0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crisis against which climate         0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crisis that has gone                 0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a critique of his writing              0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a critiquing service for a             0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crowd at baltimore county            0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a crucial liberal myth the             0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a cup of coffee tea                    0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a cute guy puts himself                0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a cute little boutique we              0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a cybersecurity corpsof highly trained 0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a daily basis i have                   0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a dan lepard recipe which              0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a dan sir horace gunderson             0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a dang ankle stop complaining          0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a danger to everyone he                0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a dangerousness to the ilsa            0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a daughter like me right               0 0 0 0 0 0 0 0 0  0  0  0  0  0
##   a day cleanse with whole               0 0 0 0 0 0 0 0 0  0  0  0  0  0
##                                         Docs
## Terms                                    15 16 17 18 19 20
##   a couple other look-at-me blogs         0  0  0  0  0  0
##   a couple really pronounced rushes       0  0  0  0  0  0
##   a couple times and get                  0  0  0  0  0  0
##   a couple what is inextricably           0  0  0  0  0  0
##   a court martial powell sided            0  0  0  0  0  0
##   a coward hides and inflicts             0  0  0  0  0  0
##   a crab dip it was                       0  0  0  0  0  0
##   a craft store to see                    0  0  0  0  0  0
##   a creative way suggestions included     0  0  0  0  0  0
##   a crib size shirt using                 0  0  0  0  0  0
##   a crime or even a                       0  0  0  0  0  0
##   a criminal lawyer now defamation        0  0  0  0  0  0
##   a criminal or civil matter              0  0  0  0  0  0
##   a crisis against which climate          0  0  0  0  0  0
##   a crisis that has gone                  0  0  0  0  0  0
##   a critique of his writing               0  0  0  0  0  0
##   a critiquing service for a              0  0  0  0  0  0
##   a crowd at baltimore county             0  0  0  0  0  0
##   a crucial liberal myth the              0  0  0  0  0  0
##   a cup of coffee tea                     0  0  0  0  0  0
##   a cute guy puts himself                 0  0  0  0  0  0
##   a cute little boutique we               0  0  0  0  0  0
##   a cybersecurity corpsof highly trained  0  0  0  0  0  0
##   a daily basis i have                    0  0  0  0  0  0
##   a dan lepard recipe which               0  0  0  0  0  0
##   a dan sir horace gunderson              0  0  0  0  0  0
##   a dang ankle stop complaining           0  0  0  0  0  0
##   a danger to everyone he                 0  0  0  0  0  0
##   a dangerousness to the ilsa             0  0  0  0  0  0
##   a daughter like me right                0  0  0  0  0  0
##   a day cleanse with whole                0  0  0  0  0  0
```

```r
mTri <- as.matrix(tdmTok5)
word_freqs_tri <- sort(rowSums(mTri), decreasing = TRUE)
dmTri <- data.frame(word = names(word_freqs_tri), freq = word_freqs_tri)
dmTri20 <- dmTri[1:20,]
## Plot bigrams
gBi <- ggplot(dmTri20, aes(reorder(word, -freq), freq))
gBi <- gBi + geom_bar(stat = "identity", fill = I("grey50"))
gBi <- gBi + labs(x = "Quintgram", y = "Frequency")
gBi <- gBi + theme(axis.text.x = element_text(angle = 90, size = 12, hjust = 1))
gBi
```

![](NLP1_files/figure-html/ngrams-5.png)

The tm package in R supplies Zipf's plot to see word frequencies are inversely proportional to their rank. A Heaps' plot is also included. Heaps' law states vocabulary increases polynomically with text size. Zipf's plot and Heaps' plot are presented below based on the same term-document matrix used for the unigram, bigram, trigram, quantgram distributions above. When the data in the corpus follow Zipf's Law, the [slope of the fitted line on a log-log plot is -1](http://www.ccs.neu.edu/home/ekanou/ISU535.09X2/Handouts/Review_Material/zipfslaw.pdf). For data following Heaps' Law, the slope of the fitted line on a log-log plot will be close to 0.5. The plots below then indicate a random sample of 100 texts from each file to create a corpus of 300 texts is not enough from which to make accurate predictions.

```r
## Zipf's plot
Zipf_plot(tdmTok)
```

![](NLP1_files/figure-html/zipf-heap-1.png)

```
## (Intercept)           x 
##    3.962298   -0.392335
```

```r
## Heaps's plot
Heaps_plot(tdmTok)
```

![](NLP1_files/figure-html/zipf-heap-2.png)

```
## (Intercept)           x 
##   0.4965445   0.9238696
```

