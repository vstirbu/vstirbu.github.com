---
layout: post
title: "Implementing a REST API for Tessel's camera module using Regio"
tags: javascript tessel rest
---

## Blueprint

Ideally the camera module should be exposed as one web resource that allows clients to retrieve images using a `GET`request:

{% highlight http %}
GET /image HTTP/1.1
Accept: image/*
{% endhighlight %}

The obvious response, if everything goes fine, is the captured image itself:

{% highlight http %}
HTTP/1.1 200 OK
Content-Type: image/jpeg

<!-- image content -->
{% endhighlight %}

However, the camera module being hosted by an embedded device we need to take into account the situation when the hardware is not yet initialized:

{% highlight http %}
HTTP/1.1 503 Service Unavailable
{% endhighlight %}

The other situation when the request cannot be completed successfully is when the camera sensor is busy due to a request originated from another client. This can be conveyed to the client by giving a hint on when it can try again. Here the value can be the observed time that was needed to provide the image, on average 10 seconds:

{% highlight http %}
HTTP/1.1 503 Service Unavailable
Retry-After: 10
{% endhighlight%}

The API can be bit smarter and queue the requests, but on Tessel the WiFi TI3300 chipset cannot handle more that 4 simultaneous open sockets and it is safer to just inform clients to try again rather than run into the situation where the board hangs.

## Implementation

[Regio](https://github.com/vstirbu/regio) is a [express](http://expressjs.com/)-like web framework developed specifically for Tessel. The API described in the blueprint will be implemented as a `router` so that it can be mounted easily into any Regio application later on.

{% highlight javascript %}
module.exports = middleware;

var states = {
  STANDBY: 0,
  BUSY: 1
};

function middleware(options) {
  var regio = require('regio'),
      tessel = require('tessel'),
      camera = require('camera-vc0706').use(tessel.port[options.port]),
      app = regio.router(),
      hwReady = false,
      current;

  camera.on('ready', function() {
    // sets the hardware ready state and sensor in stand-by when
    // camera module is initialized
    hwReady = true;
    current = states.STANDBY;
  });

  app.get('/', handler);

  ...

  return app;
}
{% endhighlight %}

The `handler` implements the three cases described in the blueprint: the image retrieval from the sensor, the sensor busy, and the case when the hardware module is not initialized.

{% highlight javascript %}
function handler(req, res) {
  if (hwReady) {
    if (current === states.STANDBY) {
      current = states.BUSY;
      camera.takePicture(function (err, image) {
        res.set('Content-Type', 'image/jpeg');
        res.set('Content-Length', image.length);
        res.status(200).end(image);
        current = states.STANDBY;
      });
    } else {
      res.set('Retry-After', 100);
      res.status(503).end();
    }
  } else {
    res.status(503).end();
  }
}
{% endhighlight %}

## Using the middleware

All the functionality to interact with the camera is encapsulated by the middleware module, and it can be easily integrated into an existing application. You just need to indicate on which port the camera module is connected... Lets say is port `A` in this case:

{% highlight javascript %}
var regio = require('regio'),
    camera = require('regio-camera'),
    app = regio();

app.use('/camera', camera({
  port: 'A'
}));

var server = app.listen(8080, function () {
  console.log('server listening on', server.address().port);
});
{% endhighlight %}

The software is available on [github](https://github.com/vstirbu/regio-camera).
