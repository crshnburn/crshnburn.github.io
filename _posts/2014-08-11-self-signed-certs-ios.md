---
layout: post
title: Connecting to a self signed cert from iOS
---

One of the key advantages of SSL/HTTPS is that the certificate authority (CA) functionality means you don't need to manage a certificate for every server you want to connect to.

On the downside, if you have a test server you don't want to go to the expense of getting an externally signed certificate when there may be only one user, so you resort to a self-signed certificate.

By default iOS applications will not connect to these servers over HTTPS but with the following snippet added to the bottom of your `AppDelegate.m` file.

``` objc
@implementation NSURLRequest(DataController)
+ (BOOL)allowsAnyHTTPSCertificateForHost:(NSString *)host
{
    return YES;
}
@end
```
