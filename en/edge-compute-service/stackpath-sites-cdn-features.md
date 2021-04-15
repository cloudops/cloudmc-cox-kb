---
title: "CDN settings for sites"
slug: edge-compute-cdn-settings
---


<!-- Summary

What can I break with each of these features?  Fortunately, nearly all of the settings function independently of each other, which makes it easier to tweak settings, measure the impact, and then decide whether to modify the setting again.  Also, most of these setting won't actually break anything, except for CORS support.
-->

The CDN feature settings are grouped into two sections.  The first section addresses cache handling, which controls how the CDN will make decisions on storing assets as well as how those assets will be delivered.  The latter section includes policies for the client browser, which control how the CDN will tell the end-user's browser to handle assets.

Users must have the *User* role or higher, and be a member of the environment, and have the environment role *Editor* or *Owner* to manage sites.

Navigate to the desired service, click on the desired environment, and click on the tab labeled *Sites* to manage your sites.

#### Cache handling

- **Lifetime:**  If this option is set to *Origin controlled*, the CDN will pass along the cache-control headers set at the origin.  If set to *Specify CDN TTL*, a popup menu will appear, from which you may select a lifetime for static assets to be stored.  This will not affect caching of dynamic content, which will still be cached according to the cache-control headers set at the origin.  If *Never expire* is selected, static content will be stored in the cache and will not be refreshed until manually purged.  Finally, the *Do not cache* setting will prevent the CDN from storing any assets in the cache, and all requests will be served from the origin. See [Sites Overview](edge-compute-sites-overview.md) for more information on asset lifetimes.
![Lifetime](../../assets/sp-cdn-lifetime-en.png)

- **Query string control:**  If this option is set to *Ignore query strings*, the query parameters in a URL will not be used as cache keys, thus `https://www.acme.com/?qp=1` and `https://www.acme.com/?qp=2` would be stored as a single asset. If *Cache all query strings* is selected, all query parameters will be tracked as cache keys, so `https://www.acme.com/?qp=1` and `https://www.acme.com/?qp=2` would be stored as two separate assets.  If this is set to *Custom*, a text box will appear where the names of one or more query parameters to use as keys for caching may be entered.  Generally, if the content your application send back to the browser is not affected by the values in your query parameters (or the presence of query parameters), then this option can be left off.
![Query string](../../assets/sp-cdn-querystring-en.png)

- **Dynamic caching by header:**  When enabled, a text box will appear where the names of one or more request headers to use as cache keys may be entered.  This causes the CDN to make caching decisions based on headers present in the request.  This allows the CDN to store different versions of an asset with the same URL by looking at the value of the specified headers.  Requests for the same asset with different values for the header will be considered unique for each value of the header.
![Dynamic caching](../../assets/sp-cdn-dynamiccaching-en.png)

- **GZIP compression:**  The HTTP protocol allows a server to compress content before sending it in the response to the client.  Enabling this option will cause the CDN to compress content that isn't already compressed by the origin.  Enabling this option will reveal a popup menu for selecting the level of compression to use, from level 1 (lightest compression) to level 6 (maximum compression).  The default is level 6.  It is important to note, upon enabling GZIP compression content already stored in the cache will not be compressed, so you may wish to purge the cache to have this option take effect immediately.
![Gzip compression](../../assets/sp-cdn-gzip-en.png)

- **Content persistence:**  If this setting is enabled, when assets stored in the cache expire but the origin isn't reachable, the assets will be conserved in the cache and served to users until the origin is available again.  This is a unique benefit of the CDN.  Enabling this setting will reveal a popup menu for selecting the maximum length of time that these stale assets will be preserved.  Once this length of time has been reached, the cache will discard the assets even if the origin is still unavailable.

- **Use Vary header:**  The `Vary` response header is used by a server to give the browser a list of headers that the server will use for creating custom responses.  Enabling this option will tell the CDN to honor the `Vary` header coming from the origin.  Care should be taken when selecting headers to include, because it can be easy to create a significant number of unnecessary unique assets on the CDN (for example, if the `User-Agent` header is in the `Vary` header).
![Vary header](../../assets/sp-cdn-vary-en.png)

- **Canonical header:**  Enabling this option tells the CDN to add the `Link` header and defines the asset as canonical (the original).  Useful for search engines and SEO, because the asset is being served through a domain name that may or may not match what's in the HTML, and including this header will tell the search engine what this document actually represents.  When this option is enabled a text box will appear where you can enter the hostname to use in the header.

- **URL caching:**  Enabling this feature allows the CDN to cache URLs without file extensions. By default, the CDN caches static content such as images, CSS, and JavaScript files.  When this is enabled, dynamic content as well as static content may be cached.
![URL caching](../../assets/sp-cdn-urlcaching-en.png)

#### Client browser policy

- **Browser cache TTL:**  Allows you to control how long the end-users's browser will store an asset in its local cache before the asset expires.  Enabling this injects the HTTP cache-control headers into responses with the specified time to live.  However, this will NOT override a cache-control header if it's already present in the response from the origin server.  The default is one hour.  
![Browser cache TTL](../../assets/sp-cdn-browsercachettl-en.png)

- **CORS header support:**  By default, a browser that downloads JavaScript from a site will not allow that script to request resources from any site other than the site the script came from.  Enabling this feature and specifying *All origins* injects the `Access-Control-Allow-Origin` header with a value of `*`, which tells the browser to allow cross-origin requests to this site from all domains.  Selecting the option *Specify origins* gives a text field for entering a list of domains for which the browser may allow cross-origin requests to this site.  Disabling this feature will allow only the default behavior, with no cross-origin requests allowed.
![CORS support](../../assets/sp-cdn-corssupport-en.png)

- **HTTP/2 support:** Enables the CDN to allow requests using the HTTP/2 protocol, which is supported by all major Web servers, CDNs, and Web browsers.  We recommend evaluating if HTTP/2 will benefit your particular application and needs.
![HTTP/2 support](../../assets/sp-cdn-http2support-en.png)

- **HTTP/2 server push:**  When enabled along with HTTP/2 support, the CDN will push assets to the browser prior to the browser requesting them.  This can help improve performance on sites with pages that have no third-party content, such that a requested page and all its assets are sent in a single response, or also on sites that have a clear likely click-through path, where most users are likely to click on a given page.  The server must be configured properly at the origin server to provide the proper Link header.
![HTTP/2 Server Push](../../assets/sp-cdn-http2serverpush-en.png)
