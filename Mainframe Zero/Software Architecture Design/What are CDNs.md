# What are CDNs in High-Performance system design
CDNs or Content Distribution and Delivery Networks are popular solution for minimizing request latency when fetching static assets from a server.
Ideal CDN is composed of group of servers spread out globally hence no matter how far away is user from your server they'll always be close to a CDN so that, user can fetch cached copies of assets from these CDNs.
## Popular CDNs
- Cloudflare CDN
- AWS Cloudfront
- GCP Cloud CDN
- Azure CDN
- Oracle CDN
## How do the CDNs work?
CDN is a globally distributed group of servers that cache static assets for your origin server. Every CDN server has its own local cache which all should be in sync. There are two primary ways for CDN cache to be populated which make distinction between push and pull CDNs.
* Push, engineers must push new or updated files to the CDN propagating to all of the CDNs server caches.
* Pull, the server cache is lazily updated

> [!FAILUR] Never use CDNs for sensitive assets
> Financial data or Government services shouldn't be saved in CDNs cache
