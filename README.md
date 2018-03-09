# data-science topic modeling - Using data science for explaining what is data science..
### Data collection

This project is based on "[Data Science Stack Exchange](https://datascience.stackexchange.com/)" - website which dedicated to questions and answers  about data science.

And "[Cross Validated](https://stats.stackexchange.com/)" which is more focused on statistics.

To extract the tags from all the posts there I ran the following query in stack exchange's Data Explorer:

``` sql
SELECT Tags 
FROM Posts
WHERE Tags IS NOT NULL
```

The query result looks like this:

```
<machine-learning><neural-network><deep-learning>
<statistics><time-series>
<machine-learning>
<python><keras><convnet><audio-recognition>
<statistics><unbalanced-classes>
```

Where each row represents a post.

### extract transform load

Convert the data into list of lists:

(we use 2 data sources: "[Data Science Stack Exchange](https://datascience.stackexchange.com/)" and "[Cross Validated](https://stats.stackexchange.com/)")

``` python
lst = []
reader = csv.reader(open('QueryResults.csv'))
for line in reader:
    lst.append(unicode(line)[3:-3].split('><'))
reader2 = csv.reader(open('QueryResults2.csv'))
for line in reader2:
    lst.append(unicode(line)[3:-3].split('><'))
```

After we converted the data into list of lists, we used ```gensim``` to format the data :

``` python
dictionary = gensim.corpora.Dictionary(lst)
corpus = [dictionary.doc2bow(gen_doc) for gen_doc in lst]
lda = gensim.models.ldamodel.LdaModel(corpus=corpus, id2word=dictionary, num_topics=8)
```

**The most important parameter here is the ```num_topics``` which determine for how many topics we want to divide the model** - too many topics will result in very narrow topics but too few may lead to ambiguous topics..  

### visualization

For visualization we used  ```pyLDAvis```

![](capture.png)







