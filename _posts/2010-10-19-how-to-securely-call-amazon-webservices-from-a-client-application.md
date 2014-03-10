---
layout: post
title: "How to securely call amazon webservices from a client application"
description: ""
category: 
tags: []
---
{% include JB/setup %}

If you are going to use the amazon sdk in order to create calls to s3 or other amazon web service, you will notice that in order to upload files or create get requests you have to work with an object that requires your accessKeyID and secretAccessKeyID.

{% highlight ruby %}
  Amazon.AWSClientFactory.CreateAmazonS3Client(accessKeyID, secretAccessKeyID)
{% endhighlight %}

The problem is that since this is a client application, you will have to give your password to client applications in order to create calls using the amazon sdk. However, this is not safe at all. Even if you encrypt your password it’s only a matter of time until some evil hacker decrypts it and you don’t want to pay for illegal stuff hosted in your bucket.

There are two solutions in order to avoid this:

1. Create a proxy that fronts the remote AWS service (however this is not recommended)

2. Create a signed request to your server, sending all the details needed so that you can generate the authentication string and then send back the authentication string to your client in order to complete the request header.
Here’s how it works

If you want to upload a file to amazon s3 this is how your request looks like:

{% highlight ruby %}
  PUT /my-image.jpg HTTP/1.1
  Host: myBucket.s3.amazonaws.com
  Date: Wed, 12 Oct 2009 17:50:00 GMT
  Authorization: AWS 15B4D3461F177624206A:xQE0diMbLRepdf3YB+FIEXAMPLE=
  Content-Type: text/plain
  Content-Length: 11434
  Expect: 100-continue
  [11434 bytes of object data]
{% endhighlight %}

We can clearly notice the only thing that authenticates our request is the following line:

{% highlight ruby %}
  Authorization: AWS 15B4D3461F177624206A:xQE0diMbLRepdf3YB+FIEXAMPLE=
{% endhighlight %}

Here is how the authorization string can be created:

{% highlight ruby %}

  Authorization = "AWS" + " " + AWSAccessKeyId + ":" + Signature;

  Signature = Base64( HMAC-SHA1( UTF-8-Encoding-Of(
                                  YourSecretAccessKeyID, StringToSign ) ) );

  StringToSign = HTTP-Verb + "\n" +
    Content-MD5 + "\n" +
    Content-Type + "\n" +
    Date + "\n" +
    CanonicalizedAmzHeaders +
    CanonicalizedResource;

  CanonicalizedResource = [ "/" + Bucket ] +
    <HTTP-Request-URI, from the protocol name up to the query string> +
    [ sub-resource, if present.
          For example "?acl", "?location", "?logging", or "?torrent"];

  CanonicalizedAmzHeaders = <described below>

{% endhighlight %}

What this means is that we just have to send the details about the file we want to upload to our website, where we will calculate the authorization string, then, we can use the authorization string to complete the request from our client application.
