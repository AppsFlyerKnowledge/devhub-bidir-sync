---
title: Steam C++
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
> Link to repository  
> [GitHub](https://github.com/AppsFlyerSDK/appsflyer-steam-sample-app)

# AppsFlyer Steam C++ SDK integration

AppsFlyer empowers gaming marketers to make better decisions by providing powerful tools that solve real pain points, including cross-platform attribution, mobile and web analytics, deep linking, fraud detection, privacy management and preservation, and more.

Game attribution requires the game to communicate with AppsFlyer APIs over HTTPS and report user activities like first opens, consecutive sessions, and in-app events. For example, purchase events.
We recommend you use this sample app as a reference for integrating the code that reports user activities in your C++. **Note**: The sample code that follows is currently only supported in a Windows environment.

**Prerequisites**:

- [Steamworks SDK](https://partner.steamgames.com/doc/sdk) integrated in your project.
- [vcpkg](https://vcpkg.io/en/index.html) openssl & nlohmann-json packages:
   <pre><code>vcpkg install nlohmann-json:x86-windows
   vcpkg install openssl:x86-windows</code></pre>

<hr/>

## AppsflyerSteamModule - Interface

AppsflyerSteamModule.h”, included in the `appsflyer-module` folder, contains the required code and logic to connect to AppsFlyer servers and report events.

### `void start(const char_ devkey, const char_ appID)`

This method receives your API key and app ID and initializes the AppsFlyer Module that sends first open and session requests to AppsFlyer.

**Usage**:

<pre><code>AppsflyerSteamModule()->start("DEV_KEY", "STEAM_APP_ID");
</code></pre>

<span id="app-details">**Arguments**:</span>

- `STEAM_APP_ID`: Found in the [SteamDB](https://steamdb.info/apps/).
- `DEV_KEY`: Get from the marketer or [AppsFlyer HQ](https://support.appsflyer.com/hc/en-us/articles/211719806-App-settings-#general-app-settings).

### `void **onCallbackSuccess**(HTTPRequestCompleted_t\* **pCallback**)`

### `void **onCallbackFailure**(HTTPRequestCompleted_t\* **pCallback**)`

The above methods are placeholders for the desired actions upon success/failure.  
It is possible to handle different types of events with the switch case of the context within each function (“FIRST_OPEN_REQUEST”, ”SESSION_REQUEST”, ”INAPP_EVENT_REQUEST”).

### `void **logEvent**(std::string **event_name**, json **event_values**)`

This method receives an event name and JSON object and sends in-app events to AppsFlyer.

**Usage**:

<pre><code>json event_values = { {"af_currency", "USD"}, {"af_price", 6.66}, {"af_revenue", 24.12} };
std::string event_name = "af_purchase";
AppsflyerSteamModule()->logEvent(event_name, event_values);
</code></pre>

**Note**: To use the JSON, make sure to use the following imports:

<pre><code>#include &lt;nlohmann/json.hpp>
using json = nlohmann::json;
</code></pre>

## Running the sample app

1. Install [Visual Studio](https://visualstudio.microsoft.com/).
2. Open the solution `../appsflyer-steam-sample-app/steam-sample-app/steamworksexample/SteamworksExample.sln`.
3. Open the `Source Files/Main.cpp` file.
4. On line 244, replace `DEV_KEY` and `STEAM_APP_ID` with your [app details](#app-details).
5. Run the app by clicking **Play** on the top toolbar (Local Windows Debugger). Make sure that the mode is set to Debug.
   ![Visual Studio Toolbar Image](images/vs-run.PNG?raw=true "Visual Studio Toolbar Image")
6. After 24 hours, the dashboard updates and shows organic and non-organic installs and in-app events.

## Implementing AppsFlyer in your Steam game

### Setup

1. Copy the files from the `appsflyer-module` folder into your C++ project under **Header Files** > **AppsFlyer**.
2. Import the Module:
   <pre><code>#include "AppsflyerSteamModule.h"
   </code></pre>
3. Import `nlohmann-json`.
   <pre><code>#include &lt;nlohmann/json.hpp>
   using json = nlohmann::json;
   </code></pre>
4. [Initialize](#void-startconst-char_-devkey-const-char_-appid) the AppsFlyer integration.
5. Report [in-app events](#void-logeventstdstring-event_name-json-event_values).