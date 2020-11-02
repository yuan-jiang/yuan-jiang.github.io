---
layout: post
title: How to import millions of records into mysql
date: 2020-09-18 19:50:00 +0900
tags: rails ruby
---

With a gem called [activerecord-import](https://github.com/zdennis/activerecord-import), importing a huge amount of records into mysql table becomes an easier task to do, and this post demonstrates how to achieve it.

# Requirement
Suppose we have a mysql table called `visitor_ids` that saves the visitor ids for some really high traffic website. The SQL to create such table:
{% highlight sql %}
CREATE TABLE `visitor_ids` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `visitor_id` varchar(255) NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
)
{% endhighlight %}
And an active record model is defined as `VisitorId`.
Now we have say more than 1 million such visitor ids needed to be inserted into this table.

# Solution
### Use CSV as the data source
{% highlight csv %}
visitor id
4B1A9AFD6614529C3C37D5F18C5E070DD34D33BFA2B7E04F70D23B0FB1945401
A72BCFA9DC6A089C5D465531C988CFEC84F83CA0159250CD5A314D67544F7DEF
...
{% endhighlight %}

### Use `activerecord-import` for bulk import
- require dependencies
{% highlight ruby %}
require 'activerecord-import/base'
require 'activerecord-import/active_record/adapters/mysql2_adapter'
{% endhighlight %}

- initialize a queue to buffer the record objects
{% highlight ruby %}
class VisitorIdsImporter
  SOURCE_CSV_FILE = '/path/to/visitor_ids.csv'
  CHUNK_SIZE = 10_000
  MAX_IMPORT_THREADS = 3

  def initialize
    @queue = Queue.new
  end
end
{% endhighlight %}

- produces the records
{% highlight ruby %}
class VisitorIdsImporter
  def build_visitor_ids
    Thread.new do
      counter = 0
      visitor_ids = []
      CSV.foreach(SOURCE_CSV_FILE, headers: true) do |row|
        visitor_ids << [row['visitor id']]
        counter += 1
        if (counter % CHUNK_SIZE).zero?
          @queue << visitor_ids
          visitor_ids = []
        end
      end
      @queue << visitor_ids unless visitor_ids.empty?
      MAX_IMPORT_THREADS.times do
        @queue << :done
      end
    end
  end
end
{% endhighlight %}

- imports the records
{% highlight ruby %}
class VisitorIdsImporter
  def import_visitor_ids
    MAX_IMPORT_THREADS.times.map do
      Thread.new do
        while (visitor_ids = @queue.pop)
          break if visitor_ids == :done

          VisitorId.import! [:visitor_id], visitor_ids, validate: false
          puts "[#{Thread.current.object_id}]: #{Time.now}: Imported #{visitor_ids.size}."
        end
      end
    end.each(&:join)
  end
end
{% endhighlight %}

- run the import
{% highlight ruby %}
class VisitorIdsImporter
  def start
    puts "#{Time.now}: Import start"
    build_visitor_ids
    import_visitor_ids
    puts "#{Time.now}: Import end"
  end
end
{% endhighlight %}

### Summary
How long does it take to import 1 million records?
It depends.  
As a reference though:  
It took around 2 minutes for an AWS instance with 16G memory and 4 CPUs to import about 1 million records into a mysql instance also hosted by AWS.
