---
layout: post
title: Java group list objects by attribute
date: 2017-10-28 09:21:00 +0800
tags: java
---

How to group a list of objects by one of the object's attribute, which is similar to SQL group by statement and having the same results.

## Object example
{% highlight java %}
@Data
class Truck {
    private Integer id;
    private String plateNumber;
    private String truckType; // 5t, 8t, 11t, and etc.
    private Integer loadCapacity;
}

List<Truck> trucks = findAll(); // get a list of Truck objects
{% endhighlight %}

## Group truck by truckType - for loop
{% highlight java %}
Map<String, List<Truck>> trucksByType = new HashMap<String, List<Truck>>();

for (Truck truck: trucks) {
    String truckType = truck.getTruckType();
    if (!trucksByType.containsKey(truckType)) {
        trucksByType.put(truckType, new ArrayList<Truck>());
    }
    trucksByType.get(truckType).add(truck);
}
{% endhighlight %}

## Group truck by truckType - stream
{% highlight java %}
trucksByType = trucks.stream().collect(Collectors.groupingBy(Truck::getTruckType()));
{% endhighlight %}

## Group truck by truckType - com.google.common.collect.Multimaps
{% highlight %}
ImmutableListMultimap<String, ImmutableList<Truck>> trucksByType = Multimaps.index(trucks, { Truck t -> return t.getTruckTypep() });
{% endhighlight %}

See here for [reference](https://stackoverflow.com/questions/21678430/group-a-list-of-objects-by-an-attribute-java)