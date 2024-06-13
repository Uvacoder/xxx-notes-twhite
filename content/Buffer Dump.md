---
dg-publish: true
---

```sh
node index.js

  Puppeteer old Headless deprecation warning:
    In the near future `headless: true` will default to the new Headless mode
    for Chrome instead of the old Headless implementation. For more
    information, please see https://developer.chrome.com/articles/new-headless/.
    Consider opting in early by passing `headless: "new"` to `puppeteer.launch()`
    If you encounter any bugs, please report them to https://github.com/puppeteer/puppeteer/issues/new/choose.

/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:114
                    ? new Error(`${response.errorText} at ${url}`)
                      ^

Error: net::ERR_TIMED_OUT at https://www.programmathically.com/computer-architecture-hardware/
    at navigate (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:114:23)
    at async Function.race (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/util/Deferred.js:82:20)
    at async Frame.goto (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:80:21)
    at async CDPPage.goto (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Page.js:651:16)
    at async main (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/index.js:10:3)

Node.js v18.0.0


https://www.programmathically.com/computer-architecture-hardware/
proxyServer
184.181.217.194	
TimeoutError: Navigation timeout of 30000 ms exceeded
headless: false
args: [ '--proxy-server=IP_HERE:PORT_HERE' ]
184.181.217.194
4145
ERR_CONNECTION_RESET
o
await page.setUserAgent('Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3419.0 Safari/537.36');
Error: net::ERR_CONNECTION_RESET
https://github.com/puppeteer/puppeteer/issues/1477#issuecomment-437568281
(async function main() {
    try {
        const browser = await puppeteer.launch({headless: true});
        const page = await browser.newPage();
        await page.setUserAgent('Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3419.0 Safari/537.36');
        await page.goto('http://example.com');
         //your code
         await browser.close();
    }
    catch(e){
        console.log(e);
    }
})();
https://proxybot.io/api/v1/API_KEY?url=www.your-target-website.com
const proxy = 'https://proxybot.io/api/v1/API_KEY?url=';
  const url = 'https://whatismyipaddress.com/';
  const pageUrl = proxy + url;
yourApiKeyHere
node index.js

  Puppeteer old Headless deprecation warning:
    In the near future `headless: true` will default to the new Headless mode
    for Chrome instead of the old Headless implementation. For more
    information, please see https://developer.chrome.com/articles/new-headless/.
    Consider opting in early by passing `headless: "new"` to `puppeteer.launch()`
    If you encounter any bugs, please report them to https://github.com/puppeteer/puppeteer/issues/new/choose.

/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:114
                    ? new Error(`${response.errorText} at ${url}`)
                      ^

Error: net::ERR_TIMED_OUT at https://www.programmathically.com/computer-architecture-hardware/
    at navigate (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:114:23)
    at async Function.race (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/util/Deferred.js:82:20)
    at async Frame.goto (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Frame.js:80:21)
    at async CDPPage.goto (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/common/Page.js:651:16)
    at async main (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/index.js:10:3)

Node.js v18.0.0
http://api.scraperapi.com?api_key=yourApiKeyHere
curl "http://api.scraperapi.com?api_key=yourApiKeyHere&url=http://httpbin.org/ip"
node index.js

  Puppeteer old Headless deprecation warning:
    In the near future `headless: true` will default to the new Headless mode
    for Chrome instead of the old Headless implementation. For more
    information, please see https://developer.chrome.com/articles/new-headless/.
    Consider opting in early by passing `headless: "new"` to `puppeteer.launch()`
    If you encounter any bugs, please report them to https://github.com/puppeteer/puppeteer/issues/new/choose.

/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/util/Deferred.js:27
                    this.reject(new Errors_js_1.TimeoutError(opts.message));
                                ^

TimeoutError: Navigation timeout of 30000 ms exceeded
    at Timeout.<anonymous> (/Volumes/Main/root/dev/personal/open-source/active/pptr-scrapers/node_modules/puppeteer-core/lib/cjs/puppeteer/util/Deferred.js:27:33)
    at listOnTimeout (node:internal/timers:564:17)
    at process.processTimers (node:internal/timers:507:7)

Node.js v18.0.0
https://www.programmathically.com/computer-architecture-hardware
  const proxy = 'http://scraperapi:yourApiKeyHere@proxy-server.scraperapi.com:8001';
  const url = 'https://www.programmathically.com/computer-architecture-hardware/';
curl -x "http://scraperapi:yourApiKeyHere@proxy-server.scraperapi.com:8001" -k "http://httpbin.org/ip"
http://scraperapi:yourApiKeyHere@proxy-server.scraperapi.com:8001
```