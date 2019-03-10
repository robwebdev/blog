# Service Worker Rendering

Service worker rendering (SWR) is an approach I adopted when building a [Hacker News PWA](https://github.com/robwebdev/inclusive-hnpwa).

SWR encourages full page reloads that are progressively enhanced by a service worker. A fetch event handler intercepts navigation requests and renders the response in the service worker. I'm calling this **Service Worker Rendering**. SWR should be used on top of server side rendering and can be used instead or as well as client side routing. It is another layer to consider when applying progressive enhancement.

## Overview

A high level view of the Service Worker Rendering (SWR):

1. A user visits the application for the first time. Their request hits the server where there is a JavaScript application running in Node.js. The request is handled, the required data is fetched from a third party API and the complete page is rendered as the response body.

2. The response is received and the page is loaded in the browser. A service worker is then requested and begins it's lifecycle in the background.

3. If an internal link is clicked before the service worker has activated then the request will be handled by the server as in step 1. The service worker script contains another instance of the application that, once booted, will intercept any request that have a [mode of `navigate`](https://developer.mozilla.org/en-US/docs/Web/API/Request/mode).

4. If an internal link is clicked after the service worker has initialised and a request is intercepted, the same process happens in the service worker as it did on the server. The required data is fetched from the third party API page is rendered, all in the service worker.

Once the service worker is activated only the data required is requested on subsequent page loads. This (theoretically) makes subsequent page loads faster which is one of the arguments _for_ client side routing and rendering. This all happens under the hood in the service worker so the browser behaves the same whether the request hits the server or is handled in the service worker. This has accessibility benefits because the default browser behaviour (focus management, loading states etc) does not have to be reimplemeneted in client side JavaScript. Having to remimplement default browser behaviour is an argument _against_ client side routing.

## Progressive enhancement

SWR aligns with progressive enhancement. If services workers aren't supported by the browser, or the service worker fails for some reason the server still acts as a fully functional application.

## Implementation

[Hacker News PWA - Inclusive Edition](https://github.com/robwebdev/inclusive-hnpwa) is a working example of a SWR implementation. It uses [preact-render-to-string](https://github.com/developit/preact-render-to-string) for rendering, express for the server side routing and [route-matcher](https://github.com/cowboy/javascript-route-matcher) for the service worker routing. There is no clent side routing in the application, in fact the only JavaScript to run in the context of the web page is to register the service worker.
