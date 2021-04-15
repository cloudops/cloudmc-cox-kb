---
title: "Overview of sites"
slug: edge-compute-sites-overview
---


### What are sites?

**Sites** are the mechanism you will use to control the delivery of your content through the network.  This includes configuration of features available through the Content Delivery Network (CDN), as well as the Web Application Firewall (WAF), and Serverless Scripting.  A site acts as a bridge between which services the requests from users via one or more *Delivery domains* on the front end, and which on the backend obtains the dynamic responses and the static *assets* for those requests from your server, called the *origin*.

This structure makes it quick and easy to improve your application performance, and also to apply settings uniformly to your static and dynamic content, all under your control.  The delivery domain will use your domain name for the front end, which increases customer confidence, eases administration, and assists with Search Engine Optimization (SEO).

#### Content Delivery Network (CDN)

As stated above, a site bridges the delivery of content from the origin to the end-user.  The heavy work of getting that content to your users is performed by the CDN.  The CDN receives requests from your end-users, and fetches them from your origin server.  Following rules that you set, the CDN will store a site's assets in caches on the Edge servers.  The global network is then able to accelerate the delivery of the assets from the cache to users, enhancing performance and user experience.

When your site is properly configured, your end-users will access your application via the CDN without their knowing that there is an intermediary.  This is because the delivery domain will use a DNS record within your domain, rather than an unrecognized domain.

For a very simplified example, let's say that https://www.acme.com is currently served from your own servers.  This includes all content, both static and dynamic.  Let's then imagine that your site becomes fully integrated with the CDN, and let's also imagine that we can watch the very first request coming in from your first end-user.  The user enters the URL for Acme into their browser bar and hits Return.  What happens?  Because your domain's DNS will be configured to point `www.acme.com` (this is your delivery domain) to say `xyz123.cdn.com` (this is your endpoint), customers accessing your site will never see the xyz123 address, since the delivery domain is set to your domain.  The CDN receives your end-user's request.  The CDN will check its cache and see that it has no assets for the requested URL, so the CDN will connect to the origin that you specified in the site configuration, and make the same request as it received from your customer.

When your server responds to the CDN, it sends back a set of HTTP headers as well as the HTML for your landing page.  The CDN immediately sends the HTML on to your end-user, and at the same time it will check the headers to see if the origin says it is allowed to cache the response.  In this case, the headers say that this response should not be cached, so that's it, the CDN will take no further action.

At this point, the browser has received the response and begins rendering the page on the end-user's screen.  The browser sees that it needs to load an image, `https://www.acme.com/image.jpeg`.  The cycle repeats: browser send the request to `www.acme.com`, the CDN sees the request, checks its cache, sees no asset for `https://www.acme.com/image.jpeg`, makes the request to the origin, the origin responds with the file, and the CDN responds to the browser with the file.  Except this time, when the CDN checks the headers, it sees that the origin allows the file to be cached, so the CDN stores `https://www.acme.com/image.jpeg` for your site on the Edge servers.

Your end-user now clicks through to a second page.  This page also includes the same image, `https://www.acme.com/image.jpeg`.  Now we see the usefulness of the CDN:  when it receives the request for this image, the CDN matches the request against the asset already stored in the cache.  We have a **cache hit**, so the CDN immediately sends back the image to the browser, eliminating the step of going all the way to the origin, and thereby freeing up the origin for other tasks.

Following this example, site features allow administrators to configure the origin where assets are pulled from, the length of time to store them in the cache, how to determine the uniqueness of an asset, as well as many other behaviors.

As you can see, there are many applications where a CDN can have a significant impact on performance.  You can put the CDN in front of your Workload, or even in front of object storage services such as S3.  The example above uses HTTPS, because the CDN allows you to upload your existing SSL certificate and private key to enable your site to support HTTPS connections.

We recommend developing a strategy for your use of caching.  The above example had a mix of cacheable and non-cacheable content, and you can configure your site to optimize the settings for your content.  Also, it is vitally important to have your Web server at the origin be configured to serve your assets with the correct headers in order to cache those assets effectively.  Depending on your organization, both your development and operations teams may have interest in how your caching strategy will impact planning and deployments.

##### Lifetimes and TTLs

One of the most important settings to consider when configuring your site is the length of time after an asset has been stored in the cache before the CDN refreshes the asset from the origin.  This is called the **lifetime**, and is frequently referred to as the TTL, or *time to live*. Higher cache TTLs will reduce the number of times the CDN will pull the assets from origin, and it also means that without manually clearing the cache, an update to an asset will take longer to propagate to end users.  This is one of the ways that your deployments might be affected by the CDN -- if you make frequent changes to your assets, you may want to have a lower lifetime so that assets refresh more quickly.  On the other hand, if your assets never change, you may certainly benefit from a longer lifetime.

TTLs are enforced wherever there are cached assets, namely at the CDN and at the end-user's browser.  An unrelated TTL that often comes into play is for DNS records, and this is controlled by a domain's name servers.  The DNS TTL does not affect the expiration of assets, only the caching of a domain name, but it can impact your planning and deployments.  It is important not to confuse the two.

##### Cache keys

When the CDN needs to differentiate between assets that could be similar or identical, **cache keys** are the criteria that make an asset unique.  The CDN will examine these criteria for each asset and then store or discard accordingly.  For example, judging by filename alone, the CDN will consider `https://www.acme.com/image1.jpeg` and `https://www.acme.com/image2.jpeg` to be different assets, which is a reasonable assumption.  But what about a different situation, where we are passing query parameters?  For example, `https://www.acme.com/?qp=1` and `https://www.acme.com/?qp=2` would be treated by default as different assets.  However, you can define different parameters as cache keys for your site, and tell the CDN that these two requests are in fact for the same asset, despite having differing values for the `qp` query parameter.

<!--
#### Web Application Firewall (WAF)

Controls the kinds of HTTP requests that can come in based on criteria such as headers and query parameters.
-->
