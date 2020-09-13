# Pagerank Project

A search engine for the website https://www.lawfareblog.com.
This website provides legal analysis on US national security issues.

## The power method

Implement the `WebGraph.power_method` function in `pagerank.py` for computing the pagerank vector.

**Part 1: Computing the pagerank vector**
```
$ python3 pagerank.py --data=./small.csv.gz --verbose
OUTPUT GOES HERE
```

**Part 2: Using `--search_query` argument**
Using`--search_query` flag that takes a string as a parameter. The program returns all urls that match the query string sorted according to their pagerank, meaning that this gives us the most important pages on the blog related to our query.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
OUTPUT GOES HERE

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
OUTPUT GOES HERE

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
OUTPUT GOES HERE
```

**Part 3: Using `--filter_ratio` argument** 
Get a list of the pages with the largest pagerank by running

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz
OUTPUT GOES HERE
```
Since these pages are not articles, we need to modify the P matrix by removing all links to non-article pages.
One way to do so is to use `--filter_ratio` argument. It will remove all pages that have more links than the specified fraction.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
OUTPUT GOES HERE
```

(When Google calculates their P matrix for the web,
they use a similar process to modify the P matrix in order to reduce spam results.)


**Part 4: Pagerank rankings when using different arguments**
The runtime of pagerank depends heavily on the eigengap of the $\bar\bar P$ matrix, and that this eigengap is bounded by the alpha parameter.


```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose 
OUTPUT GOES HERE

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
OUTPUT GOES HERE

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
OUTPUT GOES HERE

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
OUTPUT GOES HERE
```


## The personalization vector

The most interesting applications of pagerank involve the personalization vector.

**Part 1: Implementing the personalization vector**
Implement the `WebGraph.make_personalization_vector` function.
`make_personalization_vector` function enables the `--personalization_vector_query` command line argument,
which provides an alternative method for searching by doing the filtering on the personalization vector.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
OUTPUT GOES HERE
```

These results are significantly different than when using the `--search_query` option:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query='corona'
OUTPUT GOES HERE
```

Which results are better depends on your definition of "better."
With the `--personalization_vector_query` option,
a webpage is important only if other coronavirus webpages also think it's important;
with the `--search_query` option,
a webpage is important if any other webpage thinks it's important.


**Part 2: Using `--personalization_vector_query` argument**
Another use of the `--personalization_vector_query` option is that we can find out what webpages are related to the subject but don't directly mention the subject.

For example, the following query ranks all webpages by their `corona` importance,
but removes webpages mentioning `corona` from the results.
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='-corona'
OUTPUT GOES HERE
```
There are many urls about concepts that are obviously related like "covid", "clinical trials", and "quarantine",
but this algorithm also finds articles about Chinese propaganda and Trump's policy decisions.
Both of these articles are highly relevant to coronavirus discussions,
but a simple keyword search for corona or related terms would not find these articles.

**Part 3: National security topics**
Looking at other national security topics.

