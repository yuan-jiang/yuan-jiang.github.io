---
layout: post
title: Android layout_weight
date: 2016-07-18 16:46:00 +0800
author: Yuan Jiang
tags: android
---

layout_weight is used in LinearLayouts to assign "importance" to Views within the layout. All Views have a default layout_weight of zero, meaning they take up only as much room on the screen as they need to be displayed. Assigning a value higher than zero will split up the rest of the available space in the parent View, according to the value of each View's layout_weight and its ratio to the overall layout_weight specified in the current layout for this and other View elements.

To give an example: let's say we have a text label and two text edit elements in a horizontal row. The label has no layout_weight specified, so it takes up the minimum space required to render. If the layout_weight of each of the two text edit elements is set to 1, the remaining width in the parent layout will be split equally between them (because we claim they are equally important). If the first one has a layout_weight of 1 and the second has a layout_weight of 2, then one third of the remaining space will be given to the first, and two thirds to the second (because we claim the second one is more important).

## Calculation formula
{% highlight text %}
space assigned to child = (child's individual weight) / (sum of weight of every child in Linear Layout)
{% endhighlight %}

## Usages

1. Horizontal orientation: child must be: layout_width="0dp"
{% highlight xml %}
<LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="horizontal">

      <Button
          android:layout_width="0dp"
          android:layout_height="wrap_content"
          android:layout_weight="1"
          android:text="button_1" />

      <Button
          android:layout_width="0dp"
          android:layout_height="wrap_content"
          android:layout_weight="2"
          android:text="button_2" />

      <Button
          android:layout_width="0dp"
          android:layout_height="wrap_content"
          android:layout_weight="1"
          android:text="button_3" />
</LinearLayout>
{% endhighlight %}

2. Vertical orientation: child must be: layout_height="0dp"
{% highlight xml %}
<LinearLayout
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:text="button_1"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:text="button_2"/>

</LinearLayout>
{% endhighlight %}

## Tips
- layout_weight only works for LinearLayout (TableLayout), not for RelativeLayout and etc.
- The weight numbers can be any valid numbers, not necessarily be integers, it's only the total that matters.

## References
- [What does android:layout_weight mean](http://stackoverflow.com/questions/3995825/what-does-androidlayout-weight-mean)
- [Andorid layouts](https://developer.android.com/guide/topics/ui/declaring-layout.html#CommonLayouts)
