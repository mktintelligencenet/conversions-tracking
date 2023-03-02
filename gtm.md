# How to use tracking with stages

Nuviad provides tracking code that can report to the campaign post-click events. These events can represent different stages of a sales funnel, different event on a page and so on.
Notice for these examples you need to include the JQuery:
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js" type="text/javascript"></script>

## Storing Click Session

The first step when tracking is to store the click id in a local cookie so we can use it when we need to track post click events. 

In GTM add the following tag, with the following firing trigger options:

* Name: **Initialization - All Pages**
* Type: **Initialization**

```html
<script type="text/javascript">
/* Tracking code start */
(function() {
    "use strict";

    var head = document.getElementsByTagName("head")[0];
    var trackingScript = document.createElement("script");

    trackingScript.setAttribute("async", true);
    trackingScript.setAttribute("src", "//static.rtbaxs.io/tracking.min.js?ts=" + new Date().getTime());

    trackingScript.addEventListener("load", function () {
        window.trackingStoreSession();
    });

    trackingScript.addEventListener("error", function () {
        console.log("Failed to load tracking SDK");
    });

    head.appendChild(trackingScript);
})();
/* Tracking code end */
</script>
```

The initial conversion tracked on our platform is stage 0 which confirms that the user got to the landing page. This stage 0 report is achieved by a transparent pixel. In your HTML `<body>` of the first landing page, please paste this transparent pixel:
```html
    <img src="https://notify.rtbaxs.io/conversion/pixel" referrerpolicy="no-referrer-when-downgrade" type="image/gif" />
```
    
## Conversion Stages

The post-click event tracker can track up to 10 events (0-9) reported by you in your html page as you see fit. As noted, these can represent different stages in your conversion funnel or different events on the landing page.

Conversion tracking usually happens when user performs some actions on your page.

Consider the following scenario. You have a product subscription process and within this process user needs to fill a registration form. Let"s imagine your conversion process consists of 3 stages:

1. Stage 0 - user loads the landing page. This is covered by the pixel above.
2. Stage 1 - e.g. user fills in the registration form. This should be added as sampled below.
3. Stage 2 - e.g. user clicks ‘Subscribe’ button. This should be added as sampled below.

To track other events/ stages you just need to change **TRACKING_CONVERSION_STAGE** variable value in quotes to a different stage e.g. **"2"**.
Numbers 1-9 can be used for the TRACKING_CONVERSION_STAGE (0 is already in use for page-load).
In GTM add the following tag. Make sure the tag fires on the proper events (e.g. on click on `Contact Us` button). 

```html
<script type="text/javascript">
/* Conversion stage tracking start */
(function() {
    "use strict";

    var TRACKING_CONVERSION_STAGE = "2";  /* Change this number according to the stage to be reported upon page load */
    if (window.trackingStoreSession !== undefined && window.trackingConversion !== undefined) {
        window.trackingStoreSession();
        window.trackingConversion(TRACKING_CONVERSION_STAGE);
    } else {
        var head = document.getElementsByTagName("head")[0];
        var trackingScript = document.createElement("script");
        
        trackingScript.setAttribute("async", true);
        trackingScript.setAttribute("src", "//static.rtbaxs.io/tracking.min.js?ts=" + new Date().getTime());
        
        trackingScript.addEventListener("load", function () {
            window.trackingStoreSession();
            window.trackingConversion(TRACKING_CONVERSION_STAGE); //use this in each following pages in case you want to track their loading
        });
        
        trackingScript.addEventListener("error", function () {
            console.log("Failed to load Hubzity tracking SDK");
        });
        
        head.appendChild(trackingScript);
    }
})();
/* Conversion stage tracking end */
</script>
```

Please note: the code mentioned in **Storing Click Session** section above **MUST BE** also added to the GTM where you want to track conversions.