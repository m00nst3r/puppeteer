# akolisnyk/puppeteer

Docker image with `puppeteer` and nodejs that running in alpine linux

`puppeteer` - v.1.4.0-next.1526593919447

## how to build

`sh ./sh/build.sh`

## Example

`index.js`

```ecmascript 6
const express = require("express");
const app = express();
const {inBrowser} = require('browser-based-export');
const PORT = process.env.PORT || 3000;

try {
    inBrowser({
        puppeteerOptions: {
            executablePath: "/node_modules/puppeteer/.local-chromium/linux-557152/chrome-linux/chrome",
            args: ["--no-sandbox"]
        },
        action: ({exportPdf}) =>
          exportPdf({
            payload: {
              url: 'https://example.com',
            },
            timeoutInSeconds: 10,
          }).then(pdfBuffer => {
              console.log("Hello, world!");
            // Do something with it.
          }),
      });
} catch(err) {
    console.log("Crash");
    console.log(err);
}

app.listen(PORT, err => {
    if (err) {
        console.log("Error");
        console.log(err);
    }
    console.log(`App is running on port: ${PORT}`);
})
```

`Dockerfile`

```shell script
FROM akolisnyk/puppeteer:1.4.0

RUN mkdir -p /home/pptruser/app

WORKDIR /home/pptruser/app

COPY ./ /home/pptruser/app

RUN npm config set PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

RUN npm install

CMD ["node", "index.js"]
```