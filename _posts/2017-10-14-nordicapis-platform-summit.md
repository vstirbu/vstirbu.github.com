---
layout: post
title: "Fresh impressions from Nordic APIs Platform Summit"
tags: conference talk nokia nordiapis
---

Seems like yesterday, when I heard from [Miro Nieminen](https://twitter.com/mironieminen) about a new [conference](https://nordicapis.com/events/2016-platform-summit/) about APIs in Stockholm. Following the tip, I attended the conference and haven't been disappointed. One year later, I've decided to try the waters of speaking at a tech conference. So, I've submitted the talk proposal on the last day. Soon after, I got the notification that my proposal has been accepted.

Now, back at home I decided to share my thoughts about this year [edition](https://nordicapis.com/events/the-2017-api-platform-summit/)... The main topics covered by the talks have been `graphql`, `serverless`, and `tooling`, which includes both processes and products like API gateways. No surprises there as this distribution follows the general trend in the industry during the past year.

Although there have been plenty of interesting talks, I'll focus on the few that caught my attention.

[Ronnie Mitra](https://nordicapis.com/sessions/programming-people-platform-beyond-conways-law/) talked about the decisions that have to be made when moving to a microservices architecture. As time and talent are limited you need to doze the energy in the right place in the organization so accelerate the move towards Microservices Harmony.

[Travis Spencer](https://nordicapis.com/speakers/travis-spencer/) provided a comprehensive overview of the API security _soup_ which included around 50 specifications, or shall I call them ingredients... [Dynamic Client Registration](https://tools.ietf.org/html/rfc7591) and [Proof Key for Code Exchange (PKCE)](https://tools.ietf.org/html/rfc7636) improve the OAuth security for public clients as they provide Proof of Possession as well as making any instance of an application unique. [Health Relationship Trust (HEART)](http://openid.net/wg/heart/) specifications are definitely something to keep an eye on if you are working in health domain especially using [FHIR](https://www.hl7.org/fhir/).

[Gunnar Berger](https://nordicapis.com/sessions/close-gap-future-promise-open-banking/) mentioned in his talk on Open Banking that publishing APIs creates _expectations_ that are not only of technical kind. They were overwhelmed by the reaction of the parties enrolling to try the APIs. This changed their perception, as they now see the APIs as means of co-creation, which brings the traditional banks in a win-win position with fintech.

[Jason Harmon](https://nordicapis.com/sessions/api-needs-single-source-truth/) provided a deep dive into how processes build around a single source of API truth improve the developer experience. Key messages here are to create the API specification first and keep one specification for each microservice in a central repository. Merge the API into a single spec using a [merge tool](https://github.com/Typeform/openapi-micro-merge) that handles the visibility and the status of the API using extensions such as `X-Visibility` and `X-Status`. Another important note is that developers are your first _community_ as they are not aware of the API internals.

[Adrian Hornsby](https://nordicapis.com/sessions/journey-towards-scaling-application-10-million-users/) talked about how to scale fast to a large user base using AWS offerings. He emphasized the importance of having the documentation clear before even stating the first line of code:

> Documentation is for us to know what we do.

[Adam DuVander](https://nordicapis.com/sessions/full-spectrum-apis-lessons-scaling-750-apis-production/) talked about te concerns that have to be addressed to cater for great user experience but also are profitable for the provider in what he coined full spectrum APIs. What an end for this year conference.

As for myself, I've [talked](https://www.slideshare.net/VladStirbu/developing-medical-grade-iot-systems-with-microservices) about the regulations and standards that make development of medical services special. I have also described how we handled these challenges. Drop me a line if you want to hear more.

Needless to say that there have been many more interesting talks. The good part is that all are available on the conference YouTube [channel](https://www.youtube.com/playlist?list=PLd2MPdlXKO10RVLkvlnNz_mmqUY9QN4dj).