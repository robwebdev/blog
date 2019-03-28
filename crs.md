# Client side routing - the trade-offs

Client-side routing has been a feature of single page app frameworks like Ember or Angular for some time. These frameworks are targeting ambitious web _applications_ that go beyond the functionality of a web _page_. There is a newer type of framework that is targeting (but not limited to) [content focused web sites](https://www.gatsbyjs.org/showcase/) like [blogs, marketing pages and e-commerce sites](https://nextjs.org/showcase/). [Gatsby](https://www.gatsbyjs.org/docs/gatsby-link/) and [Next](https://github.com/zeit/next.js/#routing) are popular examples of this type of framework and they both provide client-side routing out of the box. So it appears that the popular opinion at the moment is that client-side routing is the right choice no matter what the type of web site you are building. But is this really the case? In this article, I am going to pay special attention to some browser behaviours that can be affected by client-side routing that can easily go unconsidered.

Before I go on, it must be said that with any framework you can theoretically opt out of client-side routing by just using plain anchors, but then why use the framework in the first place?

## Loading states

By default, when a link is clicked in a web page it is treated as a navigation request and the browser enters a loading state. In Chrome a desktop, this replaces the favicon in the browser tab with an animated spinner for the duration of the page load. When the page has finished loading in Chrome, the spinner is replaced by the new pages favicon (and the page title should change in the tab - more on that later). In Safari on iOS, a blue progress bar appears at the top of the browser window as well as a spinner in the status bar. During the loading state, the current web page remains interactive so that another link can be clicked to cancel the currently running transition and load the different page. Users can also hit the escape key to cancel the page transition and remain on the current page if caught early enough.

When an application relies on client side routing the requests to fetch data during a page transition are generally made using XHR/ajax or WebSockets. When requests are made this way the browser doesn't treat it as page load, meaning it does not enter into its own loading state as it otherwise would for a navigation request. In the past, developers have gone to great lengths to try and [fake this behaviour with iframe hacks](https://stackoverflow.com/questions/1918218/how-to-have-ajax-trigger-the-browsers-loading-indicator). More commonly developers reimplement this visual behaviour inside the user interface either with a custom implementation or using a library such as [nprogress](http://ricostacruz.com/nprogress/).

Assistive technology can also hook into the default browser behaviour and communicate changes in state. If a page load is taking a while due to a slow connection, VoiceOver on MacOS will announce the progress, for example, "31 percent loaded".

### Page titles

### Focus management

### Scroll position
