---
layout: post
title: RabbitMQ Java Client
date: 2015-12-08 16:05 +0800
author: Andy Jiang
---

{% highlight java %}
ConnectionFactory factory = new ConnectionFactory();
factory.setHost(rabbitHost);
factory.setPort(rabbitPort);
factory.setVirtualHost(rabbitVhost);
factory.setUsername(rabbitUser);
factory.setPassword(rabbitPassword);
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();
{% endhighlight %}

Refer to RabbitMQ [JavaDoc](https://www.rabbitmq.com/releases/rabbitmq-java-client/v3.5.6/rabbitmq-java-client-javadoc-3.5.6/) for more.
