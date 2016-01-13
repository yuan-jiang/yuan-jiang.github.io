---
layout: post
title: ELK getting started notes
date: 2015-12-31 21:45:00 +0800
author: Yuan Jiang
tags: devops
---

Getting started notes on ELK (Elasticsearch, Logstash, Kibana).

# Logstash - collect and filter logs

## Installation:
  1. logstash-xxx.tar.gz or logstash-xxx.zip  
  - extract the package  
  - prepare a xxx.conf file  
  - cd logstash-xxx
    - bin/logstash agent -f xxx.conf  
    - nohup bin/logstash -f xxx.conf &  
    - bin/logstash -e 'input { stdin {} } output { stdout { codec => rubydebug } }'  
  2. deb/rpm package or apt-get/yum install => recommended
  - prepare xxx.conf under /etc/logstash/conf.d
    - service logstash start/stop/restart/configtest/status
    - /etc/init.d/logstash start/stop/restart/status

## Conf file
{% highlight ruby %}
  input {
    xxx {}
    ...
  }

  filter {
    yyy {}
    ...
  }

  output {
    zzz {}
    ...
  }
{% endhighlight %}

## Common plugins
  - input:  
    - [file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html)
    - [exec](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-exec.html)
    - [lumberjack](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-lumberjack.html)
    - [stdin](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-stdin.html)
    - [syslog](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-syslog.html)
    - [redis](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-redis.html)
    - [tcp](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-tcp.html)
    - [and more](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)
  - filter:
    - [grok](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)
    - [multiline](https://www.elastic.co/guide/en/logstash/current/plugins-filters-multiline.html)
    - [mutate](https://www.elastic.co/guide/en/logstash/current/plugins-filters-mutate.html)
    - [drop](https://www.elastic.co/guide/en/logstash/current/plugins-filters-drop.html)
    - [kv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-kv.html)
    - [ruby](https://www.elastic.co/guide/en/logstash/current/plugins-filters-ruby.html)
    - [and more](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)
  - output:
    - [elasticsearch](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html)
    - [stdout](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-stdout.html)
    - [nagios](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-nagios.html)
    - [tcp](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-tcp.html)
    - [redis](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-redis.html)
    - [kafka](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-kafka.html)
    - [pagerduty](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-pagerduty.html)
    - [email](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-email.html)
    - [and more](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)
  - codec:
    - [multiline](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-multiline.html)
    - [json_lines](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-json_lines.html)
    - [rubydebug](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-rubydebug.html)
    - [plain](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-plain.html)
    - [and more](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html)

## Plugin management
  - cd logstash-xxx
  - bin/plugin install/uninstall/update/list
  - bin/plugin install xxx (from rubygems)
  - bin/plugin install /path/to/xxx-plugin.gem (from local gem)

## Logging
  - /var/log/logstash/logstash.err
  - /var/log/logstash/logstash.log
  - /var/log/logstash/logstash.stdout

## Tips
  By default logstash is running under 'logstash' user and it will encounter permission
  issues when accessing some system files such as syslog:  
  {% highlight bash %}
  $ usermod -a -G adm logstash
  {% endhighlight %}
  or
  {% highlight bash %}
  $ setfacl -m u:logstash:r /var/log/syslog
  {% endhighlight %}

## Docs
  - [Logstash book](http://www.logstashbook.com/)
  - [Logstash official guide](https://www.elastic.co/guide/en/logstash/current/index.html)
  - [ELKstack ebook](http://kibana.logstash.es/content/)

## DSL
  - Logstash::Event
  - Reference field: [field], [field][field], ...
  - Data value type: bool(true/false)/string/number/array/hash
  - Conditionals and expression:
    - ==,!=,<,>,<=,>=
    - =~ (regex match), !~ (regex nonmatch)
    - in, not in
    - and, or, nand, xor
    - (), !()

## Useful links
  - [Logstash grok patterns 1](https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns)
  - [Logstash grok patterns 2](http://grokdebug.herokuapp.com/patterns)
  - [Logstash grok debugger](http://grokdebug.herokuapp.com/)
  - [Logstash grok constructor](http://grokconstructor.appspot.com/)

# Elasticsearch - index and store logs

## Installation
  1. elasticsearch-xxx.tar.gz or elasticsearch-xxx.zip
  - extract the package
  - cd elasticsearch-xxx && bin/elasticsearch (-d for daemon process)
  - curl -X GET http://localhost:9200
  2. deb/rpm package or apt-get/yum install => recommended
  - edit config if necessary: vi /etc/elasticsearch/elasticsearch.yml
  - service elasticsearch start/stop/restart/status

## Logging
  - /var/log/elasticsearch/elasticsearch.log

## Docs
  - [Elasticsearch official guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

# Kibana - view and search logs

## Installation
  - download kibana-xxx.tar.gz or kibana-xxx.zip
  - extract the package
  - cd kibana-xxx/config/ && vi kibana.yml
  - set "elasticsearch.url" (e.g. http://localhost:9200)
  - cd kibana-xxx/ && nohup bin/kibana &
  - open browser to: http://localhost:5601

## Logging
  - kibana-xxx/nohup.out

## Docs
  - [Kibana official guide](https://www.elastic.co/guide/en/kibana/current/index.html)
