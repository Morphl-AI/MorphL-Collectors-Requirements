## MorphL - Google Analytics Tracking Settings

Please see below the steps required for setting up client ID tracking in Google Analytics & Google Tag Manager and creating API credentials for the Google Analytics Reporting API (v4).

## Step 1 - Loading Google Analytics through Google Tag Manager

If you haven't used Google Tag Manager (GTM) before, it's like a container script that can load various scripts on the page, including Google Analytics.

- First step is to set up GTM, here is a [tutorial](https://support.google.com/tagmanager/answer/6103696?hl=en) on how to do this.

- Once the GTM account is created, you can add the Google Analytics tag to it and replace the analytics script on the website with the GTM script: [https://support.google.com/analytics/answer/6163791?hl=en](https://support.google.com/analytics/answer/6163791?hl=en). If you use other scripts on the website (for ex. Facebook Pixel), you can load all of those through GTM. Analytics tracking should continue functioning like before.

## Step 2 - Adding client ID, session ID and hit timestamp as custom dimensions

From the Google Analytics admin panel, go to the **Admin** section. In the **Property** section, go to **Custom Definitions > Custom Dimensions**. Add the following dimensions:

- **ClientID** with the **User** scope
- **SessionID** with the **Session** scope
- **HitTimestamp** with the **Hit** scope

-> insert screenshot

## Step 3 - Setting up tracking from Google Tag Manager

Go to the GTM admin panel and follow the steps below. The indications below are derived from [this tutorial](https://www.simoahava.com/analytics/improve-data-collection-with-four-custom-dimensions/#3-session-id) where you can find a detailed explanation about the client ID, session ID and hit timestamp.

### a) Client ID

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name "*Set Client ID in Dimension 1*" and choose the **Custom JavaScript** variable type.

Insert the code below and save it:

```
function() {
  // Modify customDimensionIndex to match the index number you want to send the data to
  var customDimensionIndex = 1;
  return function(model) {
    model.set('dimension' + customDimensionIndex, 'GA' + String(model.get('clientId')));
  }
}
```

---> screenshot

### b) Session ID

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name "*Random Session ID*" and choose the **Custom JavaScript** variable type.

Insert the code below and save it:

```
function() {
    return new Date().getTime() + '.' + Math.random().toString(36).substring(5);
}
```

This will create a random session ID.

---> screenshot

### c) Hit Timestamp

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name "*Hit Timestamp Local Time With Offset*" and choose the **Custom JavaScript** variable type.

Insert the code below and save it:

```
function() {
    // Get local time as ISO string with offset at the end
    var now = new Date();
    var tzo = -now.getTimezoneOffset();
    var dif = tzo >= 0 ? '+' : '-';
    var pad = function(num) {
        var norm = Math.abs(Math.floor(num));
        return (norm < 10 ? '0' : '') + norm;
    };
  
    return now.getFullYear() 
        + '-' + pad(now.getMonth()+1)
        + '-' + pad(now.getDate())
        + 'T' + pad(now.getHours())
        + ':' + pad(now.getMinutes()) 
        + ':' + pad(now.getSeconds())
        + '.' + pad(now.getMilliseconds())
        + dif + pad(tzo / 60) 
        + ':' + pad(tzo % 60);
}
```

---> screenshot

### d) Google Analytics Custom Variable

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name **GASettings** and choose the **Google Analytics Settings** variable type.

Set the **Tracking ID** value as your view ID from Google Analytics (should be something like *UI-xxxxx-01*) and set the **Cookie Domain** to *auto*.

-> screenshot

### e) Google Analytics Tag

It's time to put everything together. From the **Tags** section, create a new **Universal Analytics** tag or modify your existing **Universal Analytics** tag (you must have a single tag of this type).

Set the **Google Analytics Settings** to your custom **GASettings** variable and check the **Enable overriding settings in this tag** checkbox.

In the **More Settings > Fields to set** section, set **cookieDomain** to *auto* and add a **customTask** field with the value of your **Set Client ID in Dimension 1** custom variable.

In the **More Settings > Custom Dimensions** section, set index **2** with your **Random Session ID** custom variable and index **3** with your **Hit Timestamp Local Time With Offset** custom variable.

--> screenshot(s)

## Step 4 - Creating Google Analytics Reporting API credentials

The last part includes creating API credentials that can be used to authenticate to the Google Analytics Reporting API and access the data. This is done from the [Google Developer Console](https://console.developers.google.com). 

### a) Set Up Google Project

Click the button to the right of the logo. A window will open and you can press the "+" button to create a new project.

From the **Dashboard** section, click the **+ENABLE APIS AND SERVICES** button. Search for *Google Analytics Reporting API* and enable it. At this point, we have enable the API, but we also need to create credentials so we can access it.

### b) Create API credentials

Going back to the main page of the Google Developers console, go to the **Credentials** tab and click the **Manage service accounts** link. This will go to the **IAM & Admin** console, where you can click the **+CREATE SERVICE ACCOUNT** button. This will open the **Create Service Account** dialog. Add a name and select the **Role** or **Project > Viewer**.

Check the **Furnish a new private key** checkbox and choose the **JSON** format. After saving the form, a JSON file with the API credentials will be downloaded. 

### c) Allow Access to Google Analytics Data

Now the Google project and API credentials are created, but we need one more step to allow read access for the service account to Google Analytics. 

From Google Analytics, go to the **Admin** section. From the **Account** section, select **User Management**. Use the same email address that was added in the previous step (**Service account ID**). Uncheck **Notify new users by email**, uncheck **Edit** from the **Permissions** section below and save the new user.
