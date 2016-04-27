---
layout: post
title: Elasticsearch python client
date: 2016-04-27 15:56:00 +0800
author: Yuan Jiang
tags: elasticsearch python
---

Elasticsearch python client and sample code for indices creation and deletion, document indexing, and searching. For elasticsearch installation, see post [elk-getting-started-notes](/elk-getting-started-notes).

Installation
{% highlight bash %}
$ sudo pip install elasticsearch
{% endhighlight %}

Indices creation and deletion
{% highlight python %}
from elasticsearch import Elasticsearch as ES

es = ES() # by default it connects to http://localhost:9200

# create index (i.e. database compared against RDBMS)
es.indices.create(index="my-index")
# ignore 400 status caused by IndexAlreadyExistsException
es.indices.create(index="my-index", ignore=400)

# delete index
# index = "*" will delete all indices
es.indices.delete(index="my-index")
{% endhighlight %}

Document indexing
{% highlight python %}
data = {
  'summary': 'hey, elasticsearch rocks!',
  'description': 'a post introducing how elasticsearch works...',
  'date_creation': datetime.now(),
  'author': 'myself'
}

es = ES()

# index    => the index created earlier
# doc_type => i.e. table compared against RDMBS
# body     => the document to index: dict, map, or json like data
# id       => the document id, if not of interest, use uuid to generate random
es.index(index="my-index", doc_type='post', body=data, id=uuid.uuid1().hex)
{% endhighlight %}

Document searching
{% highlight python %}
es = ES()

# q        => the query to search in elasticsearch, use "*" to search all
# Other options like:
# index    => the index to search in, leave empty means search in all indices
# doc_type => the doc_type to search in, leave empty means search in all types
results = es.search(q="some-query")

# get count of hits
hits_total = results['hits']['total']

# get list of hits, where each hit is a dict
hits_list = results['hits']['hits']
{% endhighlight %}

Other common operations
{% highlight python %}
# list current elasticsearch cluster info
es.info()

# list all indices
es.cat.indices()
{% endhighlight %}

See [Python Elasticsearch Client Docs](https://elasticsearch-py.readthedocs.org/en/master/index.html)
