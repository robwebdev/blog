# Client side routing - the trade-offs

Yes, this is going to be about accessibility.

## Client-side out of the box

Client-side routing has been a feature of single page app frameworks like Ember or Angular for some time. These frameworks are targeting ambitious web _applications_ that go beyond the functionality of a web _page_. There is a newer type of framework that is targeting (but not limited to) [content focused web sites](https://www.gatsbyjs.org/showcase/) like [blogs, marketing pages and e-commerce sites](https://nextjs.org/showcase/). [Gatsby](https://www.gatsbyjs.org/docs/gatsby-link/) and [Next](https://github.com/zeit/next.js/#routing) are popular examples of this type of framework and they both provide client-side routing out of the box. So it appears that the popular opinion at the moment is that client-side routing is the right choice no matter what the type of web site you are building. But is this really the case? In this article, I am going to pay special attention to some browser behaviours that can be affected by client-side routing that can easily go unconsidered.

Before I go on, it must be said that with any framework you can theoretically opt out of client-side routing by just using plain anchors, but then why use the framework in the first place?

## Loading states

By default, when a link is clicked in a web page it is treated as a navigation request and the browser enters a loading state. In Chrome a desktop, this replaces the favicon in the browser tab with an animated spinner for the duration of the page load. When the page has finished loading in Chrome, the spinner is replaced by the new pages favicon (and the page title should change in the tab - more on that later). In Safari on iOS, a blue progress bar appears at the top of the browser window as well as a spinner in the status bar.

When an application relies on client side routing the requests to fetch data during a page transition are generally made using XHR/fetch or WebSockets. When requests are made this way the browser doesn't treat it as page load, meaning it does not enter into its own loading state as it otherwise would for a navigation request. In the past, developers have gone to great lengths to try and [fake this behaviour with iframe hacks](https://stackoverflow.com/questions/1918218/how-to-have-ajax-trigger-the-browsers-loading-indicator). More commonly developers reimplement this visual behaviour inside the user interface either with a custom implementation or using a library such as [nprogress](http://ricostacruz.com/nprogress/).

Assistive technology can also hook into the default browser behaviour and announce the progress of a slow loading page. In my test with VoiceOver on MacOS, the progress was announced after a few seconds as "31 percent loaded". To match this behaviour with client-side routing you would need to listen to a [ProgressEvent](https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent) and thoughtfully update an [invisible live region](https://inclusive-components.design/notifications/#invisibleliveregions) with a progress message. I say thoughtfully because it would probably be overwhelming to announce every time the progress updates but should happen at least once every so often in order to be informative enough to the user, at least that's how VoiceOver behaved in my test.

### Don't break the stop button

[Don't break the back button](https://ux.stackexchange.com/questions/42392/does-dont-break-the-back-button-apply-to-web-applications) is some routing related advice I heard some time ago - and it's good advice. Correctly using `history.pushState` will ensure this and that's what frameworks tend to do. But how about the stop button? During a slow page load, users can hit the browsers stop button to cancel if they get fed up waiting. This is a pretty handy default browser behaviour, especially when on a flaky mobile connection. When I click on a link that takes more than a few seconds to begin loading, I use that as a sign that I have poor signal and I hit the stop button. At this point, I am able to continue reading the page that I was already on whilst keeping an eye out for a better signal. Maybe I should put my phone back in my pocket and get a life, though.

### Don't break the escape key

## Page titles

## Focus management

## Scroll position
