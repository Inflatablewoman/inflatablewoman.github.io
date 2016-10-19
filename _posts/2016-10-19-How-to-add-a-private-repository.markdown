---
layout: post
title:  "How to add a private docker registry to k8s"
date:   2016-10-19 07:00:00
categories: k8s
---

Adding a private reigstry for use in k8s.  Hosted on ubunutu.

Add the secret
{% highlight bash %}
kubectl create secret docker-registry dockerkey --docker-username={USER} --docker-password={PASSWORD} --docker-email=r{EMAIL} --docker-server={SERVER}:{PORT}
{% endhighlight %}

On the hosts get the self signed cert (if needed):
{% highlight bash %}
ex +'/BEGIN CERTIFICATE/,/END CERTIFICATE/p' <(echo | openssl s_client -showcerts -connect {SERVER}:{PORT}) -scq > cert.crt
{% endhighlight %}

Add the cert to the list.

{% highlight bash %}
cp dev-reg.crt /usr/local/share/ca-certificates
{% endhighlight %}

Update the ca-certificates. 
 
List catalog:
{% highlight bash %}
sudo update-ca-certificates
{% endhighlight %}

Check the cert is trusted:

{% highlight bash %}
curl https://{SERVER}:{PORT}
{% endhighlight %}