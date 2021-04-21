# waddankeri-pyspark-finalproject
This project is based on displaying the word count in a file using PySpark and Databricks in cloud.

# Author
Soumya Chidambar Rao Waddankeri

# Tools and Languages
Tools: Pyspark, Databricks Notebook, Pandas, MatPlotLib, Seaborn
Language : Python

# Input Source
https://www.gutenberg.org/
[The Project Gutenberg eBook of A Tale of Two Cities, by Charles Dickens](https://www.gutenberg.org/files/98/98-0.txt)

# Databricks Published Link
https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/887981635895222/3611044710192474/6066245626354476/latest.html

# Process
## Gathering Data
## Cleansing the Data
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

# Conclusion 
The top 15 words in the book of A Tale of Two Cities, by Charles Dickens are {said, mr, one, lorry, upon, defarge, man, littleWords, time, hand, miss, know, two, good, like}. We can say that the author is mainly focussing on individuals(example:defarge) and focusses on work and time.

# References
[The Project Gutenberg eBook of A Tale of Two Cities, by Charles Dickens](https://www.gutenberg.org/files/98/98-0.txt)
[Word Count Lab: Building a word count application](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/4574377819293972/2246755934805346/3186223000943570/latest.html)
[Stack Overflow - Get the top 15 words using Spark/Python](https://stackoverflow.com/questions/59240504/spark-python-reducebykey-then-find-top-10-most-frequent-words-and-frequencies)
[Get top 5 largest from list of tuples - Python](https://stackoverflow.com/questions/41306684/get-top-5-largest-from-list-of-tuples-python/41306701)
