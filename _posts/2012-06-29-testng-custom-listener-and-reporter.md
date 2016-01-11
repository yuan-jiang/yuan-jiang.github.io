---
layout: post
title: Testng custom listener and reporter
date: 2012-06-29 13:45:00 +0800
author: Yuan Jiang
tags: java automation
---

Testng custom listener to log individual result of test method execution and custom reporter to log summary report of test suite execution.

TestNGCustomListener:
{% highlight java %}
public class TestNGCustomListener extends TestListenerAdapter
{
    private static final Logger RESULT = Logger.getLogger("TEST-RESULT");

    @Override
    public void onTestFailure(ITestResult tr)
    {
        RESULT.error(tr.getName() + "   [FAILED] \n" + StackTraceReader.readStackTrace(tr.getThrowable()));
    }

    @Override
    public void onTestSkipped(ITestResult tr)
    {
        RESULT.error(tr.getName() + "   [SKIPPED] \n" + StackTraceReader.readStackTrace(tr.getThrowable()));
    }

    @Override
    public void onTestSuccess(ITestResult tr)
    {
        RESULT.info(tr.getName() + "   [PASSED] ");
    }
}
{% endhighlight %}

TestNGCustomReporter:
{% highlight java %}
public class TestNGCustomReporter implements IReporter
{
    private static final Logger REPORT = Logger.getLogger("TEST-REPORT");

    @Override
    public void generateReport(List <XmlSuite> xmlSuites, List<ISuite> suites, String outputDirectory)
    {

        for (ISuite suite: suites)
        {
            String suiteName = suite.getName();

            int passedCount = 0;
            int skippedCount = 0;
            int failedCount = 0;

            for (ISuiteResult sr: suite.getResults().values())
            {
                passedCount += sr.getTestContext().getPassedTests().getAllResults().size();
                skippedCount += sr.getTestContext().getSkippedTests().getAllResults().size();
                failedCount += sr.getTestContext().getFailedTests().getAllResults().size();
            }

            int totalCount = passedCount + skippedCount + failedCount;

            String passed = "Passed: " + passedCount;
            String skipped = "Skipped: " + skippedCount;
            String failed = "Failed: " + failedCount;
            String total = "Total tests run: " + totalCount;

            String message = suiteName + ", " + total + ", " + passed + ", " + skipped + ", " + failed;

            if (skippedCount>0 || failedCount>0)
            {
                REPORT.info("[FAIL] " + message);
            }
            else
            {
                REPORT.info("[PASS] " + message);
            }

        }

    }
}
{% endhighlight %}

Config in testng.xml to use the custom listener and reporter:
{% highlight xml %}
<suite name="Test Suite Name" verbose="1">
    <listeners>
        <listener class-name="com.xxx.TestNGCustomListener"></listener>
        <listener class-name="com.xxx.TestNGCustomReporter"></listener>
    </listeners>

    <test name="Test Name" preserve-order="true">
        ...
    </test>
</suite>
{% endhighlight %}
