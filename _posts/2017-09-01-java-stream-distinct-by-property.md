---
layout: post
title: Java stream distinct by property
date: 2017-09-01 10:49:00 +0800
tags: java
---

Java 8 stream has a distinct() method which could be used to filter out a list of distinct objects, but the distinctness of that method is based on Object.equals(Object). What if you want to filter a list of objects based on any property (field) of the object? The [StreamEx](https://github.com/amaembo/streamex) library comes as an elegant solution.

{% highlight java %}
import lombok.Data;
import lombok.EqualsAndHashCode;
import javax.util.streamex.StreamEx;

@Data
@EqualsAndHashCode(of={"id"})
public class SomeObject {
    private Long id; // for hashCode and equals
    private Long externalId; // some id from external system, could be duplicated
    //...
    private String otherFields; 
}

//...

public List<SomeObject> getDistinctSomeObjects(List<SomeObject> someObjects) {
    return StreamEx.of(someObjects).distinct(SomeObject::getExternalId).toList();
}
{% endhighlight %}

## References
- [Java 8 Distinct by property](https://stackoverflow.com/questions/23699371/java-8-distinct-by-property)
- [StreamEx API](http://amaembo.github.io/streamex/javadoc/)


