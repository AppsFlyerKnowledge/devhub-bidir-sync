---
title: Smart Script Quick Start - Single Key
description: Recipe Description
hidden: false
recipe:
  color: '#018FF4'
  icon: 🦉
---
```javascript HTML code
<!DOCTYPE html>
<html>
<head>
  <base herf="/">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="row">
    <div class="column">
      <img src="../images/appsflyerlogo.png" alt="AppsFlyer logo">
    </div>
    <div class="column" style="text-align:right;">
      <img src="../images/onelinklogo.png" alt="OneLink logo">
    </div>
  </div>
  <h1 class="primary-heading">
    OneLink Smart Script V2
  </h1>
  <h2 class="secondary-heading">
    Single Key Demo
  </h2>
  <div class="url-container">
    <div class="input_url-container">
      <h2>Input URL</h2>
      <textarea id="input_url" class="present_url">
        No input URL
      </textarea>
    </div>
    <div class="output_url-container">
      <div id="my_qr_code_div_id"></div>
      <h2>Output URL</h2>
      <textarea id="output_url" class="present_url">
        No output from script
      </textarea>
    </div>
  </div>
  <div class="stores-wrapper">
    <a id="ios_link" href="https://apps.apple.com/us/app/my-device-id-by-appsflyer/id1192323960">
        <img class="appstore-image" src="../images/app_store.png" alt="app store link" >
    </a>
    
    <a id="andrd_link" class="appstore-image" href="https://play.google.com/store/apps/details?id=com.appsflyer.android.deviceid">
        <img src="../images/play_store.png" alt="play store link">
    </a>
  </div>
  <script type="text/javascript" src="../scripts/onelink-smart-script-v2.1.0.js"></script>
  <script>

    // load the input URL to thr input_url textArea
    document.getElementById('input_url').innerHTML = window.location.href;

    //Initializing Smart Script arguments
    var oneLinkURL = "https://engmntqa.onelink.me/LtRd/";
    var mediaSource = {keys: ["inmedia"]};
    var campaign = {keys: ["incmp"]};

    //Calling the function after embedding the code will be through a global parameter on the window object called window.AF_SMART_SCRIPT
    //Onelink URL is generated
    var result = window.AF_SMART_SCRIPT.generateOneLinkURL({
      oneLinkURL,
      afParameters:{
        mediaSource: mediaSource,
        campaign: campaign,
      }
    })

    var result_url = "No output from script"
    if (result) {
          result_url = result.clickURL;            
          document.getElementById('andrd_link').setAttribute('href', result_url);
          document.getElementById('ios_link').setAttribute('href', result_url);
          window.AF_SMART_SCRIPT.displayQrCode("my_qr_code_div_id");
    }      
    document.getElementById('output_url').innerHTML = result_url;
  </script>
</body>

</body>
</html>
```

# Import Smart Script code

<!-- javascript@46 -->

In this case the code is embedded locally

# Implement call to Smart Script in JS section

<!-- javascript@47-75 -->



# Initialize outgoing OneLink URL

<!-- javascript@53 -->

The URL must:
1. Start with `https`
2. Domain in a OneLink or branded domain
3. The URL ends with a OneLink template ID followed by a trailing `/`

# Initialize arguments with configuration object

<!-- javascript@54-55 -->

Description of Smart Script arguments can be found [here](https://dev.appsflyer.com/hc/docs/onelink-smart-script-v2web-to-app-url-generator#arguments)
The arguments is configured using a configuration object described [here](https://dev.appsflyer.com/hc/docs/onelink-smart-script-v2web-to-app-url-generator#configuration-object).

More are useful use case examples are [here](https://dev.appsflyer.com/hc/docs/onelink-smart-script-v2web-to-app-url-generator#examples)

# Call Smart Script generate method

<!-- javascript@59-65 -->

Call `generateOneLinkURL` method in the path `window.AF_SMART_SCRIPT.generateOneLinkURL`. The method prototype is:
```
var result = window.AF_SMART_SCRIPT.generateOneLinkURL({
  oneLinkURL,
  afParameters,
  referrerSkipList, // optional
  urlSkipList // optional
})
``` 
The arguments you initialized previously are placed in `afParameters`.
The method returns a `string` with the outgoing OneLink URL. Save it a variable

# Check if the result is valid

<!-- javascript@68 -->

If the result is `null` the script has an encountered an error, or skipped it operation. 
The reason can be found in the page logs

# [Optional] Place the link behind a button

<!-- javascript@70-71 -->

In some cases it will be useful to place the generated OneLink URL behind a unified *Download the app* button, or two separate buttons like in this example

# [Optional] Create a QR code from the generated link

<!-- javascript@30,72 -->

Calling the method `displayQrCode` in the path `window.AF_SMART_SCRIPT.displayQrCode` will generate a QR code in a `div` passed to the method. The `div` is created before the call to the method.

Learn more [here](https://dev.appsflyer.com/hc/docs/onelink-smart-script-v2web-to-app-url-generator#create-a-qr-code-with-the-smart-script-result)