# Client side routing - what are the tradeoffs?

## Loading states

When a link is clicked in a web page the browser enters a loading state. In Chrome on desktop, this replaces the favicon in the browser tab with an animated spinner for the duration of the page load. When the page has finished loading in Chrome, the spinner is replaced by the new pages favicon (and the page title should change in the tab - more on that later). In Safari on iOS a blue progress bar appears at the top of the browser window as well as a spinner in the status bar. During the loading state, the current web page remains interactive so that another link can be clicked to cancel the currently running transition and load the different page. Users can also hit the escape key to cancel the page transition and remain on the current page, if caught early enough.

When an application implements client side routing the network requests to fetch data during a page transition are generally made using XHR or WebSockets. When requests are made this way the browser doesn't treat it as page load, meaning it does not enter into loading state described above. In the past, developers have gone to great lengths to try and [fake this behaviour with iframe hacks](https://stackoverflow.com/questions/1918218/how-to-have-ajax-trigger-the-browsers-loading-indicator). More common is to reimpliment this visual behaviour in a custom way specific to the application. This can mean adding a loading animation or spinner somewhere within the page. An alternative approach is to display a 'skeleton UI' which hints at what the fully loaded UI will ook like while you wait.

Assitive technology can also hook into the default browser behaviour and communicate changes in state. If a page load is taking a while due to a slow connection, VoiceOver on MacOS will announce the progress, for example "31 percent loaded".

### Page titles

### Focus management

### Scroll position
