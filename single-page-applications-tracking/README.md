## MorphL - Single Page Applications (SPAs)

Tracking page views with Google Analytics on Single Page Applications can be tricky. 

On a classic website, page views are tracked whenever the analytics script is loaded, while navigating from page to page. On a SPA, the analytics script is included only once (when the index of the app is loaded) so page views need to be tracked manually as new content is loaded. Without this step, the bounce rate will significantly increase, since the bounce rate is calculated as the percentage of single page visits.

Please see [here](https://developers.google.com/analytics/devguides/collection/analyticsjs/single-page-applications) a more detailed description about the analytics script and SPAs.

Below we have added a list of resources that can help you include Google Analytics in your SPA:

1) Angular JS - [https://github.com/angulartics/angulartics](https://github.com/angulartics/angulartics)

2) React - [https://github.com/react-ga/react-ga](https://github.com/react-ga/react-ga)

3) Vue.js - [https://github.com/MatteoGabriele/vue-analytics](https://github.com/MatteoGabriele/vue-analytics)

4) Polymer - [https://medium.com/@mvuksano/loading-google-analytics-using-polymer-bce77754de5e](https://medium.com/@mvuksano/loading-google-analytics-using-polymer-bce77754de5e)