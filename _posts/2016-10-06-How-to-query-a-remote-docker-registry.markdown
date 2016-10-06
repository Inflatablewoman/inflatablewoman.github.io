---
layout: post
title:  "How to query a remote docker registry"
date:   2016-04-14 14:30:00
categories: thoughts
---

If you are having problems with kubernetes reporting ErrImageNotFound when pulling an image, you can eliminate the obvious by checking the registry in the following way.

Do a docker login:

{% highlight bash %}
docker login -u {USERNAME} -p {PASSWORD} {REGISTRY}
{% endhighlight %}

Get your docker authentication token.  Take the value for your registry url:

{% highlight bash %}
cat ~/.docker/config.json
{% endhighlight %}

Do some curl operations.  
 
List catalog:
{% highlight bash %}
curl -X GET -H "Authorization: Basic {YOURTOKEN}" "https://{REGISTRY}/v2/_catalog"
{% endhighlight %}

List tags on an image:
{% highlight bash %}
curl -X GET -H "Authorization: Basic {YOURTOKEN}" "https://{REGISTRY}/v2/{IMAGENAME}/tags/list"
{% endhighlight %}