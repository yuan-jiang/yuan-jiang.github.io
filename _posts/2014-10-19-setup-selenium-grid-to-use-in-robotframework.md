---
layout: post
title: Setup selenium grid to use in robotframework
date: 2014-10-19 14:15:00 +0800
author: Yuan Jiang
tags: automation selenium robotframework
---

Setup selenium grid to use in robotframework for web browser automation testing.

# Prerequisites
 - Download [selenium-server-standalone-x.xx.x.jar](https://selenium-release.storage.googleapis.com/index.html)
 - Grid server (hub): java
 - Grid client (node): java, robotframework, web browsers such as firefox, chrome, or ie

# Start the hub
{% highlight bash %}
$ java -jar selenium-server-standalone-x.xx.x.jar -role hub
{% endhighlight %}

# Register a node
{% highlight bash %}
$ java -jar selenium-server-standalone-x.xx.x.jar -role node  -hub http://hub-server:4444/grid/register
{% endhighlight %}

# Optional config
 - -port XXXX (default 4444)
 - -host
 - -timeout (default 300, seconds before hub releases node if no requests)
 - -maxSession (default 5, max number of browsers to run in parallel)
 - -browser
   - browserName={android, chrome, firefox, htmlunit, internet explorer, iphone, opera}
   - version={browser version}
   - firefox_binary={path to executable binary}
   - chrome_binary={path to executable binary}
   - maxInstances={maximum number of browsers of this type}
   - platform={WINDOWS, LINUX, MAC}
 - -registerCycle
 - examples:
{% highlight bash %}
$ -browser browserName=firefox,version=3.6,maxInstances=5,platform=LINUX
$ -browser browserName=firefox,version=3.6,firefox_binary=/home/myhomedir/firefox36/firefox,maxInstances=3,platform=LINUX -browser browserName=firefox,version=4,firefox_binary=/home/myhomedir/firefox4/firefox,maxInstances=4,platform=LINUX
$ -browser “browserName=firefox,version=3.6,firefox_binary=c:\Program Files\firefox ,maxInstances=3, platform=WINDOWS”
{% endhighlight %}


# Check grid console with web browser
{% highlight bash %}
$ http://hub-server:4444/grid/console
{% endhighlight %}

# Finally use the grid setup for web automation testing in robotframework script
{% highlight robotframework %}
*** Settings ***
Documentation               This is just an example
Metadata                    VERSION     1.0
Library                     Selenium2Library
Suite Setup                 Start Browser
Suite Teardown              Close Browser

*** Variables ***
${SERVER}                   http://www.google.com
${BROWSER}                  firefox

*** Keywords ***
Start Browser
    [Documentation]         Start firefox browser on selenium grid
    Open Browser            ${SERVER}   ${BROWSER}   None  http://hub-server:4444/wd/hub

*** Test Cases ***
Check page title
    [Documentation]         Check the page title
    Title Should Be         Google
{% endhighlight %}

# References
 - [Selenium grid wiki](https://github.com/SeleniumHQ/selenium/wiki/Grid2)
 - [Selenium grid quick start](http://www.seleniumhq.org/docs/07_selenium_grid.jsp#quick-start)
 - [Robotframeowrk selenium2library webdemo](https://bitbucket.org/robotframework/webdemo/wiki/Home)
