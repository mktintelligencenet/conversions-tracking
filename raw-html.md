# How to use Hubzity tracking with stages

Hubzity provides tracking code that can report to the campaign post-click events. These events can represent different stages of a sales funnel, different event on a page and so on.
Notice for these examples you need to include the JQuery:
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js" type="text/javascript"></script>

## Storing Click Session

The first step when tracking is to store the click id in a local cookie so we can use it when we need to track post click events.
Just paste the following code in your html header:

```html
<script type="text/javascript">
/* Hubzity tracking code start */
(function() {
    "use strict";

    var head = document.getElementsByTagName("head")[0];
    var trackingScript = document.createElement("script");

    trackingScript.setAttribute("async", true);
    trackingScript.setAttribute("src", "//static.rtbaxs.io/tracking.min.js?ts=" + new Date().getTime());

    trackingScript.addEventListener("load", function () {
        window.trackingStoreSession();
        // window.trackingConversion("0");
    });

    trackingScript.addEventListener("error", function () {
        console.log('Failed to load Hubzity tracking SDK');
    });

    head.appendChild(trackingScript);
})();
/* Hubzity tracking code end */
</script>
```

The initial conversion tracked on our platform is stage 0 which confirms that the user got to the landing page. This stage 0 report is achieved by a transparent pixel. In your HTML `<body>` of the first landing page, please paste this transparent pixel:
```html
    <img src="https://notify.rtbaxs.io/conversion/pixel" referrerpolicy="no-referrer-when-downgrade" type="image/gif" />
```


## Conversion Stages

The post-click event tracker can track up to 10 events (0-9) reported by you in your html page as you see fit. As noted, these can represent different stages in your conversion funnel or different events on the landing page.

Conversion tracking usually happens when user performs some actions on your page.

Consider the following scenario. You have a product subscription process and within this process user needs to fill a registration form. Let's imagine your conversion process consists of 3 stages:

1. Stage 0 - user loads the landing page. This is covered by the pixel above.
2. Stage 1 - e.g. user fills in the registration form. This should be added as sampled below.
3. Stage 2 - e.g. user clicks ‘Subscribe’ button. This should be added as sampled below.

To enable conversion tracking with multiple stages (for example clicking on a button, filing a registration form etc.), add also the following code to the event handlers which reflect user actions on your page. Numbers 1-9 can be used for the CONVERSION_STAGE (0 is already in use for load).
Your webmaster or site developer should know how handle it. 
For example, if your site has a ‘Subscribe’ button, the code could look like this:
Make sure you put this code where you actually want it to be executed, and replace the class btn-subscribe with your own in order to invoke the reporting on this stage...

### Example 1. Conversion stage is bound to a non-link (`<a href="/">`) element click

HTML:
```html
<button class="btn-subscribe" onclick="subscribe();">Subscribe now</button>
```

JS
```js
$('.btn-subscribe').on('click', function() {
    /* Hubzity conversion stage tracking start */
    var CONVERSION_STAGE = "2";
    window.trackingConversion(CONVERSION_STAGE);
    /* Hubzity conversion stage tracking end */
});
```

### Example 2. Conversion stage is bound to a link (`<a href="/">`) element click

HTML:
```html
<a class="link-subscribe" href="/subscribe">Subscribe now</a>
```

JS:
```js
$('.link-subscribe').on('click', function(e) {
    /* Hubzity conversion stage tracking start */
    e.preventDefault();
    var CONVERSION_STAGE = "2";
    window.trackingConversion(CONVERSION_STAGE, false, function() {
        document.location = e.target.href;
    });
    /* Hubzity conversion stage tracking end */
});
```

**IMPORTANT:** please use this example for link elements (`<a>`). By default on click event link element starts navigation to a different page. In order to let tracking code run correctly we add a little delay to give the code a chance finish its job before browser opens a new page.

To track other stages you just need to change **CONVERSION_STAGE** variable value in quotes to a different stage e.g. **"1"**.

Please note: the code mentioned in the above **Storing Click Session** section **MUST BE** also added to **each** page where you want to track conversions.
