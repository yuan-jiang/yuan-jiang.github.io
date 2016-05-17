---
layout: post
title: Maven simple guide
date: 2012-06-03 16:49:00 +0800
author: Yuan Jiang
tags: maven java
---

Apache maven simple guide for installation, configuration, and usage.

## Installation and configuration
- Download the [package](https://maven.apache.org/download.cgi)
- Set env variable M2_HOME = MAVEN_EXTRACTED_DIR
- Add executable to system path: $M2_HOME/bin

## mvn help:system
- Get help for sub command
- Setup local repository for local storage
- Customize local repo location: conf/settings.xml -> localRepository

## Config pom.xml
- Dependency configuration - GAV:
{% highlight xml %}
...
<groupId>com.xxx.xxx</groupId>
<artifactId>myapp</artifactId>
<version>1.0</version>
...

<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.10</version>
    <scope>test</scope>
  </dependency>
  ...
</dependencies>
{% endhighlight %}
- Common commands:
  + mvn compile
  + mvn clean
  + mvn package
  + mvn install
  + mvn test

## eclipse maven plugin
- m2eclipse
- Window -> Preferences -> Maven -> Installations -> Add -> local_maven -> OK
- Create maven project:
  + New -> Project -> Other -> Maven -> Maven project -> Next
  + maven-archetype-quickstart-1.1.jar
  + Modify jvm memory args: eclipse.ini -Xms512m -Xmx1024m

## Dependency resolution
- Pass-on: A->B; B->C; => A->C
- Conflict:
  + Same level: the earliest wins;
  + Diff level: the shortest wins;
- Explicit exclusion:
{% highlight xml %}
<exclusions>
  <exclusion>
    ...
  </exclusion>
</exclusions>
{% endhighlight %}

## Aggregation and inheritance
- Aggregation
{% highlight xml %}
...GAV...
<packaging>pom</packaging>

<modules>
  <module>.../other</module>
  ...
  <module>.../etc</module>
</modules>
{% endhighlight %}

- Inheritance
{% highlight xml %}
<dependencyManagement>
  <dependencies>
    <dependency>
    </dependency>
  <dependencies>
</dependencyManagement>

<parent>
  ...GAV...
  <relativePath>...</relativePath>
</parent>

<artifactId>...</artifactId>
{% endhighlight %}

## Repositories
- [MVN Repository](http://mvnrepository.com/)
- [Maven Central](http://search.maven.org/)
