---
layout: post
title: Passing parameters to testng xml from ant on jenkins
author: Andy Jiang
date: 2015-11-24 08:00:00 +0800
tags: java automation
---

With Jenkins “This build is parameterized” feature, we can dynamically pass testng parameters (i.e. test config or data input) through Ant.

Here is how-to:

1. In Jenkins, config job and enable “This build is parameterised” and config a set of variables;
   Note: These variables are available as env. vars and should be accessed by os shell as well as by tools like Ant and etc.

2. In Jenkins, config job build with Ant and upon job execution, the parameters configured in step 1 are automatically appended to Ant command as properties:
{% highlight bash %}
  $ ant -Dvar1=value1 -Dvar2=value2 … -DvarN=valueN runTest
{% endhighlight %}

3. In testng.xml file:
   - Do not define <parameter name=“” value=“”></parameter> for the above variables that are intended to be parameterised in Jenkins;
   - If defined in testng.xml file, the values defined in Jenkins will be overriden by those in testing xml file

4. Important step: in Ant build.xml when defining the <testng> target, ensure the “delegateCommandSystemProperties” attribute is set to “true”, e.g.
{% highlight xml %}
  <target name="runTest" depends="compile">
    <testng classpath="${cp}:${build.dir}" delegateCommandSystemProperties="true">
      <xmlfileset dir="${test-cases.dir}" includes="testng.xml"/>
    </testng>
  </target>
{% endhighlight %}

5. Important notice: In order for jenkins/ant to pass the parameters to testng, the parameters in testng java methods should be defined with @Parameters tag (The ITestContext will not work), refer to [discussion](https://groups.google.com/forum/#!topic/testng-users/_F9geWyxUsI) for more.
