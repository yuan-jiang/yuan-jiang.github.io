---
layout: post
title: Log4j testng reporter appender
date: 2012-06-15 09:00:00 +0800
author: Yuan Jiang
tags: java automation testng log4j
---

When developing test automation framework in java with testng and log4j, one can append log4j logs to the "Reporter output" section of testng html report and this post shows how to achieve that.

Step 1 - Write a custom log4j appender for testng
{% highlight java %}
public class TestNGReportAppender extends AppenderSkeleton
{

    @Override
    protected void append(final LoggingEvent event)
    {
        Reporter.log(eventToString(event));
    }

    private String eventToString(final LoggingEvent event)
    {
        final StringBuilder result = new StringBuilder(layout.format(event));

        if(layout.ignoresThrowable())
        {
            final String[] s = event.getThrowableStrRep();
            if (s != null)
            {
                for (final String value : s)
                {
                    result.append(value).append(Layout.LINE_SEP);
                }
            }
        }
        return result.toString() + "</br>"; // add a line break since the output is html format
    }

    @Override
    public void close()
    {
    }

    @Override
    public boolean requiresLayout()
    {
        return true;
    }
}
{% endhighlight %}

Step 2 - Config log4j properties to use the custom appender
{% highlight properties %}
log4j.rootLogger=DEBUG, stdout, R, testNG

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%-d{yyyy-MM-dd   HH:mm:ss}   [%c]-[%p]   %m%n

log4j.appender.R=org.apache.log4j.RollingFileAppender
log4j.appender.R.File=logs/test.log
log4j.appender.R.MaxFileSize=500KB
log4j.appender.R.MaxBackupIndex=3
log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%-d{yyyy-MM-dd   HH:mm:ss}   [%c]-[%p]   %m%n

log4j.appender.testNG=com.xxx.TestNGReportAppender
log4j.appender.testNG.layout=org.apache.log4j.PatternLayout
log4j.appender.testNG.layout.ConversionPattern=%-d{yyyy-MM-dd   HH:mm:ss}   [%c]-[%p]   %m%n
{% endhighlight %}
