# pyppeteer_test

https://github.com/Zenika/alpine-chrome
```
docker container run -d -p 9222:9222 zenika/alpine-chrome --no-sandbox --remote-debugging-address=0.0.0.0 --remote-debugging-port=9222

docker stats 713e5827fd2e
```

chrome
```
http://localhost:9222/json
```

https://pptr.dev/#?product=Puppeteer&version=v1.9.0&show=api-puppeteerconnectoptions
https://github.com/GoogleChrome/puppeteer

https://github.com/miyakogi/pyppeteer
```python

import pyppeteer
import asyncio


async def main():
    browser = await pyppeteer.connect(
        options=dict(browserWSEndpoint='ws://localhost:9222/devtools/page/452B6D013ED53884A41D1CB576D70585')
    )
    page = await browser.newPage()
    await page.goto('http://example.com')
    await page.screenshot({'path': 'example.png'})

    print(1)
    await page.close()

asyncio.get_event_loop().run_until_complete(main())
```


requets_html
```python
import pyppeteer
import asyncio

import requests
from requests_html import HTMLSession


def connect_to_chrome(session: HTMLSession) -> None:

    browser_ws = requests.get('http://localhost:9222/json').json()[0].get('webSocketDebuggerUrl')
    session.loop = asyncio.get_event_loop()
    session._browser = session.loop.run_until_complete(
        pyppeteer.connect(options=dict(browserWSEndpoint=browser_ws, ignoreHTTPSErrors=True))
    )
```
