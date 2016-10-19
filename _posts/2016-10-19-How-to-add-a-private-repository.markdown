---
layout: post
title:  "How to add a private docker registry to k8s"
date:   2016-10-19 07:00:00
categories: k8s
---

If you are getting problem with ImagePullBackOff and the detailed error:

Failed to pull image "{MYREPO/myservice}": Error response from daemon: {"message":"Get https://{SERVER}:{PORT}/v1/_ping: x509: certificate signed by unknown authority"}

There are a couple of things you can do.  The easy way:

{% highlight bash %}
vi /etc/docker/daemon.json
{% endhighlight %}

{% highlight json %}
{
  "insecure-registries": ["{SERVER}:{PORT}"]
}
{% endhighlight %}

{% highlight bash %}
systemctl restart docker
{% endhighlight %}

Or add the certifcate to the trusted list.  This example is for ubuntu.

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