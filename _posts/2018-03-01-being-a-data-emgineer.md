---
layout: post
title: "Being a data engineer in 2018"
tags: big-data gcp cloud
---

Thanks to the disturbances in the arctic vortex, yesterday was one of the [coldest day of the winter](http://www.helsinkitimes.fi/finland/finland-news/domestic/15355-finns-told-to-brace-for-cold-weather.html) so far... What better day than to head to Helsinki to [Predict What’s Next](https://cloudplatformonline.com/2018-OnBoard-Helsinki.html) with Google.

The location of the training was [Helsinki Congress Paasitorni](https://goo.gl/maps/CaGXa4p76q42), a venue that can accomodate several hundreds of participants and is easily accessible form the main railway station. The snacks and lunch kept us inside, while the two dozens or so organizing staff were present everywhere and were assisting the participants at all times. All this attention to details just shows that the battles in the clouds are hard won on the ground by winning the hearts of the developers (and the pockets of their organizations).

The content of the training was build around <q>Big Data and Machine Learning</q> theme, although in practice it covered a wide range of items starting with an overview of the Google Cloud Platform (GCP) infrastructure and how to create compute instances there. This is quite understandable considering such a large audience that might not be familiar with the platform and its tools.

The training continued with the tools available to import and persist data, with an emphasis of choosing the storage service suitable for your data volume and usage pattern. While the basics were in place, the data being stored in [Cloud SQL](https://cloud.google.com/sql/), we moved on to transforming the data using the Hadoop services provided as part of [Cloud Dataproc](https://cloud.google.com/dataproc/). A nice surprise here were the interactive experiments performed using [Datalab](https://cloud.google.com/datalab/) notebooks, which should be very familiar to [Jupyter](http://jupyter.org) users.

The afternoon part of the training was dedicated to machine learning. We were using [BigQuery](https://cloud.google.com/bigquery/) to create a dataset for a taxi demand forecast system that used the dataset to train a model with [TensorFlow](https://www.tensorflow.org). The later part of the training was an overview of the pre-trained models that are offered as [Machine Learning APIs](https://cloud.google.com/products/machine-learning/). The very end (and very brief) was allocated for the overview of data processing architecture, including managed message queues [Cloud Pub/Sub](https://cloud.google.com/pubsub/) and stream and batch processing using [Cloud Dataflow](https://cloud.google.com/dataflow/).

During the event, the trainer even mentioned that they believe that they have the greatest machine learning _infrastructure_ in the world. Point taken, considering dedicated machine learning hardware such as [Cloud TPU](https://cloud.google.com/tpu/), although AWS or Azure cannot be far behind...

Although the content covered was quite large for one day, the trainer did a good job at emphasizing the key concepts behind each service and how to use them. This made it easy to map them to the equivalent technologies and services popular outside GCP. Secondly, as the training followed two [Qwiklabs](https://qwiklabs.com) labs, it would be pretty straight forward to go deeper and get your hands dirty with the expanded seven labs that have been delivered after the training. Again, hats off to Google for organizing this event, it was a day well spent.

I'll conclude the post with a personal perspective... as someone that [build data pipelines for analysing bio-medical signals](https://www.nokia.com/en_int/news/releases/2016/06/21/nokia-technologies-to-collaborate-with-helsinki-university-hospital-and-university-of-helsinki-to-create-innovative-solutions-for-outpatient-care), I can say that with services like these  __everyone can be a data engineer__ today, the open question is how long till _everyone can be a data scientist_, but there is promise there [too](https://cloud.google.com/automl/)...
