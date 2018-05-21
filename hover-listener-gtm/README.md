## MorphL - Hover Listener for Google Tag Manager

Pushes events to the data layer about hover interactions that occur over the given CSS selectors in the page. This is for a listener only. It has no visible effect on your site and does not send data to Google Analytics or other tools unless you create additional triggers and tags to do so.


## Setup Instructions

#### 1. Download Container File

Download the `hover-listener.json` file.


#### 2. Import JSON File into GTM
Log into your own Google Tag Manager container and head to the Admin section of the site. Under Container options, select Import Container. Check out this blog post for more details about importing a container file.

#### 3. Customize Items to Track
Edit the variable “Constant – Hover Listener CSS Selectors” with a list of CSS selectors, separated by commas, on which you want to listen for hover activity.

#### 4. Create Your Own Tags, Triggers, and Variables.
This particular plugin won’t actually create any tags for you. Create the appropriate Variables to pull out information from the data layer push. You’ll need to create your own trigger using the Custom Event = hover, and then attach that to any tags that you would like to fire.

#### 4. Preview & Publish
Use the Preview options to test this container on your own site. Try testing each of the events to make sure they’re working properly. If everything looks good, go ahead and publish!

## Acknowledgements

This GTM plugin has been created by the folks at [LunaMetrics](http://www.lunametrics.com/), a digital marketing & Google Analytics consultancy. 