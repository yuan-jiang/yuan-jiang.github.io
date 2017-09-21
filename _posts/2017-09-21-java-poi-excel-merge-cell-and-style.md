---
layout: post
title: Java POI excel merge cell and style
date: 2017-09-21 14:14:00 +0800
tags: poi java
---

Quick code example on how to merge cells with POI and apply cell style to the merged cell.

## Create cell
{% highlight java %}
Workbook workbook = new HSSFWorkbook();
Sheet sheet = workbook.createSheet("new sheet");

Row row = sheet.createRow((short) 1);
row.setHeightInPoints(30); // set row height
sheet.setColumnWidth(1, 10*256); // set specific column width

Cell cell = row.createCell((short) 1);
cell.setCellValue("This is a string");

// cell.setCellType(Cell.CELL_TYPE_NUMERIC);
// cell.setCellValue(200);
{% endhighlight %}

## Add merge region
{% highlight java %}
CellRangeAddress mergedCell = new CellRangeAddress(
        1, //first row (0-based)
        1, //last row  (0-based)
        1, //first column (0-based)
        2  //last column  (0-based)
)
sheet.addMergedRegion(mergedCell);
{% endhighlight %}

## Set merged region borders
{% highlight java %}
RegionUtil.setBorderTop(CellStyle.BORDER_MEDIUM, mergedCell, sheet, workbook);
RegionUtil.setBorderBottom(CellStyle.BORDER_MEDIUM, mergedCell, sheet, workbook);
RegionUtil.setBorderLeft(CellStyle.BORDER_THIN, mergedCell, sheet, workbook);
RegionUtil.setBorderRight(CellStyle.BORDER_THIN, mergedCell, sheet, workbook);
{% endhighlight %}

## Create cell style
{% highlight java %}
CellStyle cellStyle = workbook.createCellStyle();

cellStyle.setVerticalAlignment(CellStyle.VERTICAL_CENTER);
cellStyle.setAlignment(CellStyle.ALIGN_CENTER);

Font cellFont = workbook.createFont();
cellFont.setBoldweight(Font.BOLDWEIGHT_NORMAL);
cellStyle.setFont(cellFont);

XSSFColor color = new XSSFColor(Color.WHITE);
// custom color with r, g, b
// XSSFColor color = new XSSFColor(new Color(146, 208, 80));
((XSSFCellStyle) cellStyle).setFillForegroundColor(color);
cellStyle.setFillPattern(CellStyle.SOLID_FOREGROUND);
{% endhighlight %}

## Set style
{% highlight java %}
cell.setCellStyle(cellStyle);
{% endhighlight %}

## References
- [POI javadocs](https://poi.apache.org/apidocs/index.html)
- [Busy Developers' Guide to HSSF and XSSF Features](https://poi.apache.org/spreadsheet/quick-guide.html#FooterPageNumbers)
- [Add border to merged cells in excel Apache poi java.?](https://stackoverflow.com/questions/13930668/add-border-to-merged-cells-in-excel-apache-poi-java)
