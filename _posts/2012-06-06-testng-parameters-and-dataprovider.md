---
layout: post
title: TestNG parameters and dataprovider
date: 2012-06-06 10:51:00 +0800
author: Yuan Jiang
tags: testng
---

TestNG parameters and dataprovider.

## Parameters
- test method:
  + required parameters:
{% highlight java %}
public class TestNGParametersTest()
{
  @Test (description = "Must pass parameters from xml")
  @Parameters ( {"username", "password"} )
  public void testParameters(String username, String password)
  {
    System.out.println("username=" + username);
    System.out.println("password=" + password);
    //...
  }
}
{% endhighlight %}

  + optional parameters:
  {% highlight java %}
  public class TestNGParametersTest()
  {
    @Test (description = "Optionally pass parameters from xml")
    @Parameters ( {"username", "password"} )
    public void testParameters(@Optional String username, @Optional String password)
    {
      System.out.println("username=" + username);
      System.out.println("password=" + password);
      //...
    }
  }
  {% endhighlight %}

  + optional parameters with default value:
  {% highlight java %}
  public class TestNGParametersTest()
  {
    @Test (description = "Optionally pass parameters from xml")
    @Parameters ( {"username", "password"} )
    public void testParameters(@Optional("root") String username, @Optional("root123") String password)
    {
      System.out.println("username=" + username);
      System.out.println("password=" + password);
      //...
    }
  }
  {% endhighlight %}

- testng.xml
{% highlight xml %}
<suite name="Test suite name" verbose="1">
<!--Global scope and available for all tests-->
<parameter name="username" value="admin"></parameter>
<parameter name="password" value="admin123"></parameter>
...

  <test name="Test case name" preserve-order="true">
  <!--Local scope and available for current test only-->
  <parameter name="username" value="basic"></parameter>
  <parameter name="password" value="basic123"></parameter>
  ...
    <classes>
      <class name="com.xxx.xxx.TestNGParametersTest">
        <methods>
          <include name="testParameters"></include>
        </methods>
      </class>
    </classes>
  </test>
  ...

</suite>
{% endhighlight %}

## DataProvider
- dataprovider with an array of array of objects:
{% highlight java %}
// define dataprovider
@DataProvider (name = "testData")
public Object[][] prepareDataForTest()
{
  return new Object[][]
  {
    { "Andy", new Integer(25) },
    { "John", new Integer(30) },
  };
}

// use data from above dataprovider
@Test (dataProvider = "testData", description = "Test something")
public void testSomething(String name, Integer age)
{
  System.out.println("Name: " + name + ", Age: " + age);
}
{% endhighlight %}

- dataprovider with an iterator of array:
{% highlight java %}
// define dataprovider
@DataProvider (name = "testData")
public Iterator<Object[]> prepareDataForTest()
{
  List<Object[]> profile = new ArrayList<Object[]>();
  profile.add(new Object[]{"Andy", new Integer(25)});
  profile.add(new Object[]{"John", new Integer(20)});
  return profile.iterator();
}

// use data from above dataprovider
@Test (dataProvider = "testData", description = "Test something")
public void testSomething(String name, Integer age)
{
  System.out.println("Name: " + name + ", Age: " + age);
}
{% endhighlight %}

## References
- [TestNG Doc](http://testng.org/doc/documentation-main.html#parameters)
- [DataProvider - Data Driven Testing with Selenium and TestNG](http://functionaltestautomation.blogspot.com/2009/10/dataprovider-data-driven-testing-with.html)
