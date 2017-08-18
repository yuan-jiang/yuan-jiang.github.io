---
layout: post
title: Spring resttemplate post json with utf-8
date: 2017-08-18 13:15:00 +0800
tags: spring java
---

In Spring framework RestTemplate is very useful in terms of sending various http requests to RESTful resources and this post shows simple examples on how to set Content-Type, Accept headers, as well as the content encoding, which is especially important when requesting with non-ascii (e.g. CJK languages) data.

## Initialize RestTemplate
{% highlight java %}
import java.nio.Charset;
import java.util.Arrays;
import org.springframework.web.client.RestTemplate;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpEntity;
import org.springframework.http.MediaType;
import org.springframework.http.converter.StringHttpMessageConverter;
import com.fasterxml.jackson.databind.ObjectMapper;

RestTemplate restTemplate = new RestTemplate();
{% endhighlight %}

## Set Content-Type
{% highlight java %}
HttpHeaders httpHeaders = new HttpHeaders();
httpHeaders.setContentType(MediaType.APPLICATION_JSON);
{% endhighlight %}

## Set Accept
{% highlight java %}
headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
headers.setAcceptCharset(Arrays.asList(Charset.forName("UTF-8")));
{% endhighlight %}

## Set encoding - option 1
{% highlight java %}
restTemplate.getMessageConverters().add(0, new StringHttpMessageConverter(Charset.forName("UTF-8")));
{% endhighlight %}

## Set encoding - option 2
{% highlight java %}
MediaType mediaType = new MediaType("application", "json", Charset.forName("UTF-8"));
httpHeaders.setContentType(mediaType);
{% endhighlight %}

## Post request
{% highlight java %}
XObject xObject = new XObject();
xObject.setFieldA("a");
xObject.setFieldB("b");
ObjectMapper objectMapper = new ObjectMapper();
String payload = objectMapper.writeValueAsString(xObject);
HttpEntity<String> httpEntity = new HttpEntity<String>(payload, httpHeaders);
YObject yObject = restTemplate.postForObject(httpUrl, httpEntity, YObject.class);
{% endhighlight %}

## References
- [RestTemplate JavaDoc](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html)
- [How can I tell RestTemplate to POST with UTF-8 encoding?](https://stackoverflow.com/questions/29392422/how-can-i-tell-resttemplate-to-post-with-utf-8-encoding)
