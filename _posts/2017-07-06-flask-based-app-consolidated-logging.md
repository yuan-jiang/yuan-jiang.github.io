---
layout: post
title: Flask based app consolidated logging
date: 2017-07-06 16:55:00 +0800
tags: flask python
---

Flask has an "app.logger" which you can use for webapp related logging, and for other libraries or packages that flask app is using usually python standard logging is used. There are issues reported that logs from various sources in flask based app sometimes get missed out or get messy. See the references section at the bottom for details about the issues. A workaround is to set global root logging config and override that from flask.


{% highlight python %}

import logging

from flask import Flask, jsonify

app = Flask(__name__)

# config global logging
fmt = logging.Formatter(
    "[%(asctime)s] [%(process)d] [%(levelname)s] %(name)s - %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S %z"
)
hlr = logging.StreamHandler()
hlr.setFormatter(fmt)
logging.getLogger().addHandler(hlr)
logging.getLogger().setLevel(logging.INFO)
app.logger.handlers = []
app.logger.propagate = True

@app.route("/")
def index():
    return jsonify({
        "message": "Hello, World!"
    })

if __name__ == "__main__":
    app.run()

{% endhighlight %}


## References
- [How should logging in Flask look like?](https://github.com/pallets/flask/issues/2023)
- [Design issue with flask.logger.create_logger().](https://github.com/pallets/flask/issues/641)
- [Global logging with flask](http://y.tsutsumi.io/global-logging-with-flask.html)