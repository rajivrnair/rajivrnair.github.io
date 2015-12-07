---
layout: post
title: How to say a REST-ful NO?
tags: [REST, API, http, status, 404, 'resource not found']
---

We had an interesting discussion at work yesterday: for a REST api, what HTTP status code to return when
a resource does not exist - a _**200 OK**_ with an empty result, a _**204 NO CONTENT**_ or a _**404 NOT FOUND**_ ?

A lively discussion resulted in a flurry of ideas and only one near-fatality. We figured it all boiled down
to what the client is asking for.

####(1) If the client is requesting for a resource, it is either _found (200)_ or _not found (400)_.
{% highlight java %}
GET /dogs/1
200 OK
{ "breed": "pomeranian", "type": "annoying" }


GET /dogs/991
404 NOT FOUND
{ "error": {"message": "dog '991' not found", "requestId": "x124jhakas"} }
{% endhighlight %}

####(2) If the client is searching for a resource and the query is valid, you either get _results (200 with a collection)_ or you get _no results (200 with an empty collection)_.
Here, the client receives a **200** in both outcomes, implying that the query was valid but nothing matching it was found. This is slightly better than returning a **204** because we might not want to send an empty response body that the client will have to handle differently (think NULL object pattern).
{% highlight java %}
GET /dogs?type=annoying
200 OK
{ "total": 2, "results": [{ "id": 2, "breed": "chihuahua" }, { "id": 1, "breed": "pomeranian"}] }


GET /dogs?fly=true
200 OK
{ "total": 0, "results": [] }
{% endhighlight %}

There's an interesting offshoot to this. What do these (arguably same) requests do?
{% highlight java %}
/dogs/991
404 NOT FOUND

/dogs?id=991
200 OK
{% endhighlight %}

If we follow the logic discussed above, we'll get a **400** for the first request and a **200** for the second.
This actually makes sense if you see the object-vs-query distinction.
Another view is that the second url should be redirected to the first (with a **3xx** status). IMHO, that's a bit inconsistent, i.e., what happens if you have more than one query parameter, i.e., _/dogs?id=991&annoying=true?_


###Nice Reads:
- [When to use HTTP status code 404 in an API](http://programmers.stackexchange.com/questions/203492/when-to-use-http-status-code-404-in-an-api)
