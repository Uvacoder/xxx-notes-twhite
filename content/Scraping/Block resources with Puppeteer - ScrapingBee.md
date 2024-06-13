---
dg-publish: true
---




> [!abstract]- Aside
>  This article will show you how to intercept and block requests with Puppeteer using the request interception API and the puppeteer extra plugin.
>  > [!note] 
> I found this article on Scraping Bee, which is attached to this note as markdown, with citations.
> 

  

---

In this article, we will take a look at how to block specific resources (HTTP requests, CSS, video, images) from loading in Puppeteer. Puppeteer is one of the most widely used tools for web scraping and automation. There are a couple of ways to block resources in Puppeteer. In this article, we will go over all the various methods we can use to block/intercept specific network requests in our automation scripts.

  

## Why block resources

  

First of all, let's take a look at why we would want to block certain resources. When we write automation scripts with Puppeteer often our page will load resources that are not necessary for our use case.

  

For instance, let's say we are writing a web scraper to crawl a couple of e-commerce sites to find a particular product and its price. The web pages of the e-commerce sites will have several images of the product that we are not interested in. The web pages might also have external javascript libraries to track user behaviour and collect analytics. These are all resources that we are not interested in but they will slow down our script. Therefore, we can block these resources from loading to make our script run faster.

  

Tired of getting blocked while scraping the web?

  

Join 20,000 users using our API to get the data they need!

  

## Blocking resources with Puppeteer’s request interception API

  

Puppeteer has a native [API method](https://devdocs.io/puppeteer/index#pagesetrequestinterceptionvalue) called `setRequestInterception` to block requests. This is the most simple way to block resources. Let’s take a look at an example of this.

  

```js
    // index.js
  const puppeteer = require('puppeteer');
  async function main() {
    const browser = await puppeteer.launch({
      headless: false
    });
    const page = await browser.newPage();
    await page.setRequestInterception(true);
    page.on('request', (request) => {
        // Block All Images
      if (request.url().endsWith('.png') || request.url().endsWith('.jpg')) {
        request.abort();
      } else {
        request.continue()
      }
    });
    await page.goto('https://www.scrapingbee.com/');
    await page.waitForTimeout(5000); // wait for 5 seconds
    await browser.close();
  }
  main();
```

  

In the example above we are importing Puppeteer and creating a new browser instance. On line 7 we are calling the `setRequestInterception` function and setting it `true`. This will let Puppeteer know that we would like to block specific resources on this page. For this example, we will go to the [scrapingbee.com](https://www.scrapingbee.com/) page and block all the images on that page.

  

Puppeteer provides us with a `request` listener function. It is a function that lets us tap into browser HTTP requests. Observe the code block in lines 8 through 14. We are listening to all the requests that are made by the browser and blocking the ones that contain an image extension. You can see that the `request.url()` function holds the URL information. We are checking if the request `URL` has any image extension like `png` or `jpeg`. The requests that contain an image extension are blocked.

  

Now, we can run the code with `node index.js` command. We will see the following output.

  

![image resources blocked](https://www.scrapingbee.com/blog/block-requests-puppeteer/image_ressources_blocked.png)

  

As you can see, all the image resources has been blocked.

  

**Blocking resources by type**

  

There are times when we would want to block specific resources by type. For example, we might want to block all media files (i.e. mp3, mp4). Video and audio files take significant time to load and slow down the script. Therefor it is a good idea to block these resources when we are writing a web scrapper. Below is an example of blocking resources by type.

  

```js
  const puppeteer = require('puppeteer');
  async function main() {
		const browser = await puppeteer.launch({
		  headless: false
		});
	const page = await browser.newPage();
	await page.setRequestInterception(true);
	page.on('request', (request) => {
			// Block All Images
	  if (request.url().endsWith('.png') || request.url().endsWith('.jpg')) {
		  request.abort();
		} else if (request.resourceType() === 'video') {
		  request.abort();
		}
		else {
		  request.continue()
	  }
	});
	  await page.goto('https://ca.news.yahoo.com/');
	  await page.waitForTimeout(5000); // wait for 5 seconds
	  await browser.close();
  }
main();
```

  

In the above example we are calling the `resourceType()` function in the `request` object. This function returns the resource type of the request. We are checking if the request resource type is a video or not. If it is a video then we are blocking it.

  

## Blocking resources with plugins

  

Next, let's take a look at how we can block resources by using Puppeteer plugins. If you are not familiar with Puppeteer plugins I highly recommend you take a look at [Puppeteer-extra](https://github.com/berstend/puppeteer-extra/tree/master/packages/puppeteer-extra) project. Puppeteer-extra is a wrapper for Puppeteer that allows you to use various useful plugins and libraries with Puppeteer.

  

To get started we first have to install Puppeteer-extra. We can do that with the following command.

  

Next, we will install the [Block Resource Plugin](https://www.npmjs.com/package/puppeteer-extra-plugin-block-resources) with the following command.

  

```bash
npm i puppeteer-extra-plugin-block-resources
```

  

Let’s create a new script and get the Block Resource Plugin setup with Puppeteer.

  

```js

const puppeteer = require('puppeteer-extra');

const blockResourcesPlugin = require('puppeteer-extra-plugin-block-resources')()

puppeteer.use(blockResourcesPlugin)

async function withPlugIn() {

const browser = await puppeteer.launch({

headless: false

});

const page = await browser.newPage();

blockResourcesPlugin.blockedTypes.add('media')

blockResourcesPlugin.blockedTypes.add('script')

await page.goto('http://www.youtube.com', {waitUntil: 'domcontentloaded'})

await page.waitForTimeout(5000); // wait for 5 seconds

await browser.close();

}

withPlugIn();

```

  

Notice in the code above, we are using the `use` function to add the plugin in Puppeteer. The Block Resource Plugin makes it easy to block resources. All we have to do is call the `blockedTypes.add()` with the appropriate parameter. In the example above we are blocking external JavaScript and media resources.

  

Here’s a list of all the different resources we can block with this plugin.

  

```markdown

- document,

- stylesheet,

- image,

- media,

- font,

- script,

- texttrack,

- xhr,

- fetch,

- eventsource,

- websocket,

- manifest,

- other

```

  

We can choose to dynamically add or remove the resources we want to block. Here’s an example

  

```js

...

blockResourcesPlugin.blockedTypes.add('script')

blockResourcesPlugin.blockedTypes.remove('stylesheet')

...

```

  

In the code above we are blocking the scripts in the page. Then on line 3 we are specifying that we do not wan to block stylesheets (CSS).

  

> The [official docs](https://www.npmjs.com/package/puppeteer-extra-plugin-block-resources#availabletypes) for Block Resource Plugin is a good place to start if you want to learn more about it.

  

## Summary

  

I hope this article gave you a broad overview of how to block resources with Puppeteer. If you would like to learn more about Puppeteer and web scraping best practices we encourage you to check out our [ScrapingBee blog](https://www.scrapingbee.com/blog/). That's all for today. See you next time.

  

![image description](https://www.scrapingbee.com/images/authors/shadid.jpeg)

  

**Shadid Haque**

  

I am a software craftsman and a coffee aficionado. Currently living in Toronto, Canada. I love everything JavaScript, Node.js, and in between. Always keen to solve interesting problems and transform complications into simplicity.

