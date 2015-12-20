---
layout: post
title: Passing testng command line arguments from ant
author: Yuan Jiang
date: 2015-11-24 16:46:00 +0800
tags: java automation
---

How to pass testng command line arguments dynamically when Ant is used for invoking testng task.

Below is the sample section of testng task target defined in Ant build.xml file:
{% highlight xml %}
  <target name="runTest" depends="compile">
      <testng classpath="${cp}:${build.dir}" groups="${groups}">
          <xmlfileset dir="${basedir}" includes="testng.xml"></xmlfileset>
      </testng>
  </target>
{% endhighlight %}

Then from command line, by using the jvmarg properties, you can provide the value of "groups" (which is a standard testng command line attribute) dynamically like this:
{% highlight bash %}
  $ ant runTest -Dgroups group1,group2,...,groupN
{% endhighlight %}

Above is equivalent to invoking testng from command line using java:
{% highlight bash %}
  $ java org.testng.TestNG -groups group1,group2,...,groupN testng.xml
{% endhighlight %}
