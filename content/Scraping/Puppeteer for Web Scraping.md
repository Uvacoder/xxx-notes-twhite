---
dg-publish: true
---

[Puppeteer API](http://pptr.dev/api/puppeteer.puppeteernode)

This is an interesting project. I am writing a tiny little script to scrape my developer personal website.

The website serves static files and is hosted on Vercel.

What are some things to think about?

1. Vercel has a hard cap of 100 builds on the free plan.
	1. So what does that mean for scraping
	2. Could you use warm IPs?
	3. What about using warm IPs behind a proxy, like SOCKS5?
	4. How would you [[How to Avoid Detection with Puppeteer - ZenRows#^479c97|avoid detection with a proxy?]]
2. Cloud security is pretty good, especially with a big VC backed company like Vercel. This would be a hard task to accomplish for [someone unexperienced in this stuff](https://www.0x8c.org/docs/reading-list) but I feel confident that asking more experienced pen testers, etc, would result in at the least, a brainstorming session.