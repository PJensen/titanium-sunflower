# ⚛ Titanium-Sunflower 

## Technologies

* Electron
* IPFS
* Node

![image](https://user-images.githubusercontent.com/54164/156901516-1603cd31-47b8-47fc-8b95-02092a4d6302.png)

# Electron Details Template Below

## Getting started

Let's get some easy things out of the way first. The very first thing you'll want to do when using this template is to change the name and appropriate data to point to you and your repo. To make it as easy as possible, all app-specific data is kept in `package.json` and this README. You will want to set all appropriate properties in the `package.json` file, such as `name`, `productName`, `appId`, `repository`, `author`, etc.

## Source Code Build

Right off the bat... there isn't one. And it's a good thing. I'm not going to go into the history of why I hate builds here, but I will just posit that you don't need one. Electron is the most cutting-edge browser with every latest feature combined with the most recent NodeJS runtime, all rolled into a single environment. If there is something you actually need, it is already there. The code you write will be the actual code that is executed.

## UI Framework

Lack of a code build doesn't keep us from using all new and familiar tools. This template already includes [Preact](https://preactjs.com/) to manage components and [htm](https://www.npmjs.com/package/htm) as a jsx alternative -- see the example in [App.js](renderer/App/App.js). Since it's Electron, you can use CSS [variables](https://css-tricks.com/guides/css-custom-properties/) and [functions](https://css-tricks.com/complete-guide-to-css-functions/). You can even manage CSS at the component level, with a helper to include the CSS into the page.

## Managing config

While there are some popular ways of handling config (or really any other app-level persistence), they have some problems. Some are not multithread enabled (remember, Electron has 2 threads by default, and a larger app can have many more than that when written well), some are 100% synchronous, and some are both those things. The config module provided in [`lib/config.js`](lib/config.js) is safe to use and ready for production. It's also ready to allow easy testing.

During development, the config file will default to being created in the root directory of your repo, allowing you easy access to it. In production, the config will be created in the user's app data directory.

## IPC

A common problem in Electron apps is needing to communicate between the main and renderer thread. However, [`electron.remote` is considered harmful](https://medium.com/@nornagon/electrons-remote-module-considered-harmful-70d69500f31). This template comes with a wrapper that allows exposing the same Promise-based API to both the main and all renderer threads while keeping your application responsive. See [`lib/isomorphic.js`](lib/isomorphic.js) and an example of how to use it in [`lib/config.js`](lib/config.js). Note that in these cases, all work is delegated to the main thread, keeping the renderer responsive and making this mechanism useful for work that must be done on the main thread anyway.

## End-to-End Testing

This template comes with an [app provider](test/lib/app-provider.js) helper to start and stop the application, provide custom config, and print all logs when a test fails. This helper instruments the application using [`puppeteer`](https://github.com/puppeteer/puppeteer#readme), so you can do any end-user centric automation.

You can get access to the Puppeteer `browser` and default `page` objects like this:

```javascript
const { start, stop } = require('./lib/app-provider.js');

(async () => {
  const { browser, page, utils } = await start();
})();
```

While there are some helpers in the provided `utils` object, you can use any tooling built for testing using Puppeteer. I recommend the wonderful [`pptr-testing-library`](https://github.com/testing-library/pptr-testing-library).

Refer to the existing [smoke tests](test/smoke.test.js) for more examples for end-to-end testing.

## Unit testing

There is no unit testing framework. Feel free to add any framework you are happy with. I recommend [`mocha`](https://www.npmjs.com/package/mocha), since it's already used to run the end-to-end tests.

## CI Build

This template comes with a [GitHub Actions](https://github.com/features/actions) workflow that will run your tests on Windows, MacOS, and Linux, create installers for all 3 operating systems, and provide them as artifacts on every build. When someone submits a pull request to your project, it will also do the same for the pull request. Whenever a tag is created, it will create a [GitHub Release](https://docs.github.com/en/enterprise/2.16/user/github/administering-a-repository/about-releases) and upload all installers to that release. Basically... everything you might expect from a build.
