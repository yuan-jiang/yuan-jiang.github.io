---
layout: post
title: Execute shell command in java
date: 2015-12-09 13:15:00 +0800
author: Andy Jiang
tags: java linux
---

Execute linux/unix command in java.

{% highlight java %}
String cmd = "uname -a";
Process p = Runtime.getRuntime().exec(cmd);
p.waitFor();

// Read the normal output
BufferedReader br1 = new BufferedReader(new InputStreamReader(p.getInputStream()));
StringBuilder out = new StringBuilder();
String line1;
while ((line1 = br1.readLine())!=null)
{
  out.append(line1);
  out.append("\n");
}

// Read the exceptional output
BufferedReader br2 = new BufferedReader(new InputStreamReader(p.getErrorStream()));
StringBuilder err = new StringBuilder();
String line2;
while ((line2 = br2.readLine())!=null)
{
  err.append(line2);
  err.append("\n");
}

System.out.println(out.toString() + err.toString());
{% endhighlight %}
