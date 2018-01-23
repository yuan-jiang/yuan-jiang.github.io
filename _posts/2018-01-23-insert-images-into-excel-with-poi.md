---
layout: post
title: Insert images into excel with POI
date: 2018-01-23 10:54:00 +0800
tags: poi java
---

How to insert images when outputting excel using POI packages in Java.

{% highlight java %}
try {
    FileInputStream images = new FileInputStream("/path/to/file.png");
    
    final Drawing drawing = sheet.createDrawingPatriarch();
    final ClientAnchor anchor = helper.createClientAnchor();
    anchor.setAnchorType(ClientAnchor.MOVE_AND_RESIZE );
    final int pictureIndex = workbook.addPicture(IOUtils.toByteArray(images), Workbook.PICTURE_TYPE_PNG);
    anchor.setCol1(1);
    anchor.setRow1(1);
    final Picture picture = drawing.createPicture(anchor, pictureIndex);
    picture.resize(1.0, 1.0);
} catch (Exception ex) {
    ex.printStackTrace();
}

{% endhighlight %}

## References
- [Insert image in column to excel using Apache POI
](https://stackoverflow.com/questions/28238078/insert-image-in-column-to-excel-using-apache-poi/28238262)
- [Busy Developers' Guide to HSSF and XSSF Features](https://poi.apache.org/spreadsheet/quick-guide.html)