<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/morphl-google-analytics.png" style="width:300px; height: auto;" />
</div>

## Google Analytics - Tracking Settings

Please see below the steps required for setting up client ID tracking in Google Analytics and creating API credentials for the Google Analytics Reporting API (v4).

## Step 1 - Adding client ID and session ID as custom dimensions

From the Google Analytics admin panel, go to the **Admin** section. In the **Property** section, go to **Custom Definitions > Custom Dimensions**. Add the following dimensions:

- **ClientID** with the **User** scope
- **SessionID** with the **Session** scope

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step2-ga-custom-dimensions.jpg" />
</div>

#### a) Client ID

From the **Custom Definitions** section, go to the **Custom Dimensions** section and click the **+New Custom Dimension** button. Add the name "*dimension1*" and choose the **User** scope.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step3-ga-js-client-id.png" />
</div>

#### b) Session ID

From the **Custom Definitions** section, go to the **Custom Dimensions** section and click the **+New Custom Dimension** button. Add the name "*dimension2*" and choose the **Session** scope.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step3-ga-js-session-id.png" />
</div>

#### c) Google Analytics Tag

Copy the following script and replace the view id (should be something like *UI-xxxxx-01*) to the one corresponding to your website. It should be placed right before the closing `<body>` tag in the page:

```JavaScript
(function (i, s, o, g, r, a, m) {
  i['GoogleAnalyticsObject'] = r;
  i[r] = i[r] || function () {
    (i[r].q = i[r].q || []).push(arguments)
  }, i[r].l = 1 * new Date();
  a = s.createElement(o), m = s.getElementsByTagName(o)[0];
  a.async = 1;
  a.src = g;
  m.parentNode.insertBefore(a, m)
})(window, document, 'script', 'https://www.google-analytics.com/analytics.js', 'ga');

ga('create', 'UA-XXXXXXXX-Y', 'auto'); // replace UA-XXXXXXXX-Y with your view id
ga(function (tracker) {
  ga('set', 'dimension1', 'GA' + String(tracker.get('clientId')));
  ga('set', 'dimension2', new Date().getTime() + '.' + Math.random().toString(36).substring(5));
});
ga('send', 'pageview');
```

## Step 2 - Creating Google Analytics Reporting API credentials

The last part includes creating API credentials that can be used to authenticate to the Google Analytics Reporting API and access the data. This is done from the [Google Developer Console](https://console.developers.google.com). 

### a) Create Google Project

Click the button to the right of the logo. A window will open and you can press the "+" button to create a new project.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step4-create-project.jpg" />
</div>

From the **Dashboard** section, click the **+ENABLE APIS AND SERVICES** button. 

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step4-enable-reporting-api-1.jpg" />
</div>

Search for *Google Analytics Reporting API* and enable it.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step4-enable-reporting-api-2.jpg" />
</div>

At this point, we have enabled the API, but we also need to create credentials so we can access the data.

### b) Create API credentials

Going back to the main page of the Google Developers console, go to the **Credentials** tab and click the **Manage service accounts** link. This will go to the **IAM & Admin** console, where you can click the **+CREATE SERVICE ACCOUNT** button. This will open the **Create Service Account** dialog. Add a name and select the **Role** or **Project > Viewer**.

Check the **Furnish a new private key** checkbox and choose the **JSON** format. After saving the form, a JSON file with the API credentials will be downloaded. 

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step4-create-service-account.jpg" />
</div>

### c) Allow Access to Google Analytics Data

Now the Google project and API credentials are created, but we need one more step to allow read access for the service account to Google Analytics. 

From Google Analytics, go to the **Admin** section. From the **Account** section, select **User Management**. Use the same email address that was added in the previous step (**Service account ID**). Uncheck **Notify new users by email**, uncheck **Edit** from the **Permissions** section below and save the new user.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs/google-analytics-tracking/step4-ga-add-service-id.jpg" />
</div>
