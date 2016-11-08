---
layout: post
titlel: Jenkins html publisher css issue
date: 2016-11-08 10:29:00 +0800
author: Yuan Jiang
tags: jenkins
---

Noticed in latest jenkins build (e.g. 2.19.2), the css style of html report published by html-publisher-plugin is not working. 

## Cause:
- This is due to [Content Security Policy in Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Configuring+Content+Security+Policy)
- By default the policy is set:
{% highlight plaintext %}
The default rule is set to: sandbox; default-src 'none'; img-src 'self'; style-src 'self';
{% endhighlight %}
- which applies:
  + No JavaScript allowed at all
  + No plugins (object/embed) allowed
  + No inline CSS, or CSS from other sites allowed
  + No images from other sites allowed No frames allowed
  + No web fonts allowed No XHR/AJAX allowed etc.

## Solution:
- Go to "Manage Jenkins" -> "Script console"
- Type and run below command:
{% highlight java %}
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")
{% endhighlight %}

See this stackoverflow for reference: [Jenkins - HTML Publisher Plugin - No CSS is displayed when report is viewed in Jenkins Server](http://stackoverflow.com/questions/35783964/jenkins-html-publisher-plugin-no-css-is-displayed-when-report-is-viewed-in-j)
