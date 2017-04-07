---
layout: post
title: Insert into mongodb with DBRef
date: 2017-04-07 12:40:00 +0800
tags: mongodb nosql python
---

How to insert a new object into mongodb using pymongo with reference to other objects using DBRef.

## In mongodb, external object reference is like below:
{% highlight json %}
{
    "external_object": {
        "$ref": "collection_where_external_object_exists",
        "$id": Object("xxxxxx")
    }
}
{% endhighlight %}

## With bson.dbref.DBRef, above data can be posted to mongodb like below:
{% highlight python %}
from bson.dbref import DBRef

{
    "external_object": DBRef(collection="collection_where_external_object_exists",
    id="xxxxxx")
}
{% endhighlight %}

## References
- [How to query through a DBRef in MongoDB/pymongo?

](http://stackoverflow.com/questions/2867513/how-to-query-through-a-dbref-in-mongodb-pymongo)
- [BSON](http://bsonspec.org/)