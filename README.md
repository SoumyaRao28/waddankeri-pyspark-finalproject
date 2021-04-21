# waddankeri-pyspark-finalproject
This project is based on displaying the word count in a file using PySpark and Databricks in cloud.

# Author
Soumya Chidambar Rao Waddankeri

# Tools and Languages
Tools: Pyspark, Databricks Notebook, Pandas, MatPlotLib, Seaborn
Language : Python

# Input Source
[The Project Gutenberg eBook of A Tale of Two Cities, by Charles Dickens](https://www.gutenberg.org/files/98/98-0.txt)

# Databricks Published Link
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/887981635895222/3611044710192474/6066245626354476/latest.html

# Process
## Gathering Data
First and foremost step performed is, we'll use the urllib.request library to pull the data. The data pulled is stored in a temporary file. Here, we are storing in a temp file called CitiesCharlesDickens.txt. The source file/text data I am using is [The Project Gutenberg eBook of A Tale of Two Cities](https://www.gutenberg.org/files/98/98-0.txt)

```
import urllib.request

urllib.request.urlretrieve("https://raw.githubusercontent.com/SoumyaRao28/waddankeri-pyspark-finalproject/main/ATaleofTwoCitiesByCharlesDickens.txt" , "/tmp/CitiesCharlesDickens.txt")
```

Now that the data is stored, we will move the data to a different location using dbutils.fs.mv. The dbutils.fs.mv works takes in two arguements. 
First parameter, is what the text file we will be moving and the second parameter is where we want the file to be moved. It is as shown below:

```
dbutils.fs.mv("file:/tmp/CitiesCharlesDickens.txt","dbfs:/data/CitiesCharlesDickens.txt")
```
The next step is to transfer the data into Spark, using sc.textfile

```
charlesDickensRDD = sc.textFile("dbfs:/data/CitiesCharlesDickens.txt")
```

## Cleansing the Data
The ebook currently has capitalization, punctuation, sentences, and stopwords.To get the word count the first step is to flatmap and get rid of capitalization and spaces.

```
# flatmap each line to words
wordsRDD = charlesDickensRDD.flatMap(lambda line : line.lower().strip().split(" "))
```
Next we remove the punctuations. Here, we make use of the re library to remove any characters that do not resemble letter.

```
import re
# remove punctutation
cleanedTokensRDD = wordsRDD.map(lambda w: re.sub(r'[^a-zA-Z]','',w))
```
Once the punctuations are removed, the next step will be removing any stopwords. This is done with the use of StopWordsRemover library from pyspark.ml.feature.
```
from pyspark.ml.feature import StopWordsRemover
remover = StopWordsRemover()
stopwords = remover.getStopWords()
charlesDickensWordsRDD=cleanedTokensRDD.filter(lambda wrds: wrds not in stopwords)
```
To remove any empty elements, we will filter out anything that resembles an empty element.
```
# removing all the empty spaces from the data
charlesDickens_RemoveSpaceRDD = charlesDickensWordsRDD.filter(lambda x: x != "")
```
## Processing the Data
We have successfully completed the cleanup process, The next step is to proccess our clean data.

To begin with, we will map our words into intermediate key-value pairs.i.e., (word,1). We move ahead with next step where we will transform the pairs into a sort of word count.
```
#maps the words to key value pairs
IKVPairsRDD= charlesDickens_RemoveSpaceRDD.map(lambda word: (word,1))

# reduceByKey() to get (word,count) results
charlesdickenswordcountRDD = IKVPairsRDD.reduceByKey(lambda acc, value: acc+value)
```
In the next step, we will use the sortbykey that lists the words in the descending order in 'A Tale of Two Cities by Charles Dickens' and printing the top 25 most used words.
```
charlesdickensResults = charlesdickenswordcountRDD.map(lambda x: (x[1], x[0])).sortByKey(False).take(25)
print(charlesdickensResults)
```
The collect() is used to retrieve all the elements from my dataset.
```
results = charlesdickenswordcountRDD.collect()
print(results)
```

## Charting the result
Pandas, MatPlotLib, and Seaborn has been used to visualize our performance.
```
import pandas as pd  
import matplotlib.pyplot as plt
import seaborn as sns

# preparing chart information
source = 'ATaleofTwoCitiesByCharlesDickens, by Charles Dickens'
title = 'Top Words in ' + source
xlabel = 'Words'
ylabel = 'Count'

df = pd.DataFrame.from_records(output, columns =[xlabel, ylabel]) 
plt.figure(figsize=(15,4))
sns.barplot(xlabel, ylabel, data=df, palette="cubehelix").set_title(title)
```
# Output
![Output](https://github.com/SoumyaRao28/waddankeri-pyspark-finalproject/blob/main/Output.PNG)
# Conclusion 
The top 15 words in the book of A Tale of Two Cities, by Charles Dickens are {said, mr, one, lorry, upon, defarge, man, littleWords, time, hand, miss, know, two, good, like}. We can say that the author is mainly focussing on individuals(example:defarge) and focusses on work and time.

# References
[The Project Gutenberg eBook of A Tale of Two Cities, by Charles Dickens](https://www.gutenberg.org/files/98/98-0.txt)

[Word Count Lab: Building a word count application](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4574377819293972/2246755934805346/3186223000943570/latest.html)

[Stack Overflow - Get the top 15 words using Spark/Python](https://stackoverflow.com/questions/59240504/spark-python-reducebykey-then-find-top-10-most-frequent-words-and-frequencies)

[Get top 5 largest from list of tuples - Python](https://stackoverflow.com/questions/41306684/get-top-5-largest-from-list-of-tuples-python/41306701)

https://github.com/Rajeshwari-Rudra/bigData-finalProject

