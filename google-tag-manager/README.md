<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/morphl-google-tag-manager.png" style="width:300px; height: auto;" />
</div>

## Google Tag Manager - Tracking Settings

Please see below the steps required for setting up client ID tracking in Google Analytics & Google Tag Manager and creating API credentials for the Google Analytics Reporting API (v4).

## Step 1 - Loading Google Analytics through Google Tag Manager

If you haven't used Google Tag Manager (GTM) before, it's like a container script that can load various scripts on the page, including Google Analytics.

- First step is to set up GTM, here is a [tutorial](https://support.google.com/tagmanager/answer/6103696?hl=en) on how to do this.

- Once the GTM account is created, you can add the Google Analytics tag to it and replace the analytics script on the website with the GTM script: [https://support.google.com/tagmanager/answer/6107124?hl=en](https://support.google.com/tagmanager/answer/6107124?hl=en). If you use other scripts on the website (for ex. Facebook Pixel), you can load all of those through GTM. Analytics tracking should continue functioning like before.

## Step 2 - Adding client ID and session ID as custom dimensions

From the Google Analytics admin panel, go to the **Admin** section. In the **Property** section, go to **Custom Definitions > Custom Dimensions**. Add the following dimensions:

- **ClientID** with the **User** scope
- **SessionID** with the **Session** scope

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step2-ga-custom-dimensions.jpg" />
</div>


## Step 3 - Setting up tracking 

Go to the GTM admin panel and follow the steps below. The indications below are derived from [this tutorial](https://www.simoahava.com/analytics/improve-data-collection-with-four-custom-dimensions/#3-session-id) where you can find a detailed explanation about the client ID and session ID.

#### a) Client ID

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

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step3-client-id.jpg" />
</div>


#### b) Session ID

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name "*Random Session ID*" and choose the **Custom JavaScript** variable type.

Insert the code below and save it:

```
function() {
    return new Date().getTime() + '.' + Math.random().toString(36).substring(5);
}
```

This will create a random session ID.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step3-session-id-1.png" />
</div>

#### c) Google Analytics Custom Variable

From the **Variables** section, go to the **User-Defined Variables** section and click the **New** button. Add the name **GASettings** and choose the **Google Analytics Settings** variable type.

Set the **Tracking ID** value as your view ID from Google Analytics (should be something like *UI-xxxxx-01*) and set the **Cookie Domain** to *auto*.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step3-ga-settings.png" />
</div>

#### e) Google Analytics Tag

It's time to put everything together. From the **Tags** section, create a new **Universal Analytics** tag or modify your existing **Universal Analytics** tag (you must have a single tag of this type).

Set the **Google Analytics Settings** to your custom **GASettings** variable and check the **Enable overriding settings in this tag** checkbox.

In the **More Settings > Fields to Set** section, set **cookieDomain** to *auto* and add a **customTask** field with the value of your **Set Client ID in Dimension 1** custom variable.

In the **More Settings > Custom Dimensions** section, set index **2** with your **Random Session ID** custom variable.

Set the **Trigger** field to "*All Pages*".

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step3-ga-tag.jpg" />
</div>

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step3-trigger.png" />
</div>

## Step 4 - Creating Google Analytics Reporting API credentials

The last part includes creating API credentials that can be used to authenticate to the Google Analytics Reporting API and access the data. This is done from the [Google Developer Console](https://console.developers.google.com). 

### a) Create Google Project

Click the button to the right of the logo. A window will open and you can press the "+" button to create a new project.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step4-create-project.jpg" />
</div>

From the **Dashboard** section, click the **+ENABLE APIS AND SERVICES** button. 

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step4-enable-reporting-api-1.jpg" />
</div>

Search for *Google Analytics Reporting API* and enable it.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step4-enable-reporting-api-2.jpg" />
</div>

At this point, we have enabled the API, but we also need to create credentials so we can access the data.

### b) Create API credentials

Going back to the main page of the Google Developers console, go to the **Credentials** tab and click the **Manage service accounts** link. This will go to the **IAM & Admin** console, where you can click the **+CREATE SERVICE ACCOUNT** button. This will open the **Create Service Account** dialog. Add a name and select the **Role** or **Project > Viewer**.

Check the **Furnish a new private key** checkbox and choose the **JSON** format. After saving the form, a JSON file with the API credentials will be downloaded. 

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step4-create-service-account.jpg" />
</div>

### c) Allow Access to Google Analytics Data

Now the Google project and API credentials are created, but we need one more step to allow read access for the service account to Google Analytics. 

From Google Analytics, go to the **Admin** section. From the **Account** section, select **User Management**. Use the same email address that was added in the previous step (**Service account ID**). Uncheck **Notify new users by email**, uncheck **Edit** from the **Permissions** section below and save the new user.

<div align="center">
    <img src="https://storage.googleapis.com/morphl-docs-community-edition/google-analytics-tracking/step4-ga-add-service-id.jpg" />
</div>
