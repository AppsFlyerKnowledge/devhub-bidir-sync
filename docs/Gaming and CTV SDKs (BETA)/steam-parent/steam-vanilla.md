---
title: Steam C++
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> Link to repository  
> [GitHub](https://github.com/AppsFlyerSDK/appsflyer-steam-sample-app)

## AppsFlyer Steam C++ SDK integration

AppsFlyer empowers gaming marketers to make better decisions by providing powerful tools that solve real pain points, including cross-platform attribution, mobile and web analytics, deep linking, fraud detection, privacy management and preservation, and more.

Game attribution requires the game to communicate with AppsFlyer APIs over HTTPS and report user activities like first opens, consecutive sessions, and in-app events. For example, purchase events.
We recommend you use this sample app as a reference for integrating the code that reports user activities in your C++. 

**Note**: The sample code that follows is currently only supported in a Windows environment.

### Prerequisites

- [Steamworks SDK](https://partner.steamgames.com/doc/sdk) integrated in your project.
- [vcpkg](https://vcpkg.io/en/index.html) openssl & nlohmann-json packages:

```c++
vcpkg install curl:x86-windows
vcpkg install nlohmann-json:x86-windows
vcpkg install openssl:x86-windows
```

<hr/>

## AppsflyerSteamModule - Interface

AppsflyerSteamModule.h”, included in the `appsflyer-module` folder, contains the required code and logic to connect to AppsFlyer servers and report events.

### Init

This method receives your API key and app ID and initializes the AppsFlyer Module.

**Method signature**

```c++
void Init(const char* devkey, const char* appID, bool collectSteamUid = true)
```

<span id="app-details">**Arguments**:</span>

- `string STEAM_APP_ID`: Found in the [SteamDB](https://steamdb.info/apps/).
- `string DEV_KEY`: Get from the marketer or [AppsFlyer HQ](https://support.appsflyer.com/hc/en-us/articles/211719806-App-settings-#general-app-settings).
- `bool collectSteamUid`: Whether to collect Steam UID or not. True by default.

**Usage**:

```c++
// for regular init
AppsflyerSteamModule()->Init(<< DEV_KEY >>, << STEAM_APP_ID >>);

// for init without reporting steam_uid
AppsflyerSteamModule()->Init(<< DEV_KEY >>, << STEAM_APP_ID >>, false);
```

### Start

This method sends first open and /session requests to AppsFlyer.

**Method signature**

```c++
void Start(bool skipFirst = false)
```

**Arguments**

- `bool skipFirst`: Determines whether or not to skip first open events and send session events. The value is false by default. If true , first open events are skipped and session events are sent. [See example](#skipFirstExample)

**Usage**:

```c++
// without the flag
AppsflyerSteamModule()->Start();

// with the flag
bool skipFirst = [SOME_CONDITION];
AppsflyerSteamModule()->Start(skipFirst);
```

### Stop

This method stops the SDK from functioning and communicating with AppsFlyer servers. It's used when implementing user opt-in/opt-out.

**Method signature**

```c++
void Stop()
```

**Usage**:

```c++
// Starting the SDK
AppsflyerSteamModule()->Start();
// ...
// Stopping the SDK, preventing further communication with AppsFlyer
AppsflyerSteamModule()->Stop();
```


### LogEvent

This method receives an event name and JSON object and sends in-app events to AppsFlyer.

**Method signature**

```c++
void LogEvent(std::string event_name, json event_values, json custom_event_values = {})
```

**Arguments**

- `std::string event_name`-
- `json event_parameters`: dictionary object which contains the [predefined event parameters](https://dev.appsflyer.com/hc/docs/ctv-log-event-event-parameters).
- `json event_custom_parameters` (non-mandatory): dictionary object which contains the any custom event parameters. For non-English values, please use [UTF-8 encoding](#to_utf8).

**Usage**:

```c++
// Setting the event values json and event name
std::string event_name = "af_purchase";
json event_parameters = { {"af_currency", "USD"}, {"af_revenue", 24.12} };
// Send LogEvent request
AppsflyerSteamModule()->LogEvent(event_name, event_parameters);

// Send LogEvent request with custom event params and UTF8 encoding (for non-English characters)
std::wstring ws = L"車B1234 こんにちは";
std::wstring ws2 = L"新人邀约购物日";
json custom_event_parameters = { 
    {"goodsName", AppsflyerSteamModule()->to_utf8(ws)}, 
    {"goodsName2", AppsflyerSteamModule()->to_utf8(ws2)} 
};
AppsflyerSteamModule()->LogEvent(event_name, event_parameters, custom_event_parameters);
```

**Note**: To use the JSON, make sure to use the following imports:

```c++
#include <nlohmann/json.hpp>
using json = nlohmann::json;
```

### SetCustomerUserId

This method sets a customer ID that enables you to cross-reference your unique ID with the AppsFlyer unique ID and other device IDs. Note: You can only use this method before calling `Start()`.
The customer ID is available in raw data reports and in the postbacks sent via API.

**Method signature**

```c++
void SetCustomerUserId(std::string cuid)
```

**Arguments**:

- `std::string cuid`: Custom user id.

**Usage**:

```c++
AppsflyerSteamModule()->Init(DEV_KEY, STEAM_APP_ID);
AppsflyerSteamModule()->SetCustomerUserId("Test-18-9-23");
AppsflyerSteamModule()->Start();
```

### OnCallbackSuccess, OnCallbackFailure

The above methods are placeholders for the desired actions upon success/failure.  
It is possible to handle different types of events with the switch case of the context within each function (“FIRST_OPEN_REQUEST”, ”SESSION_REQUEST”, ”INAPP_EVENT_REQUEST”).

**Method signature**

```c++
void OnCallbackSuccess(long responseCode, uint64 context)
void OnCallbackFailure(long responseCode, uint64 context)
```

### GetAppsFlyerUID

Get AppsFlyer's unique device ID. The SDK generates an AppsFlyer unique device ID upon app installation. When the SDK is started, this ID is recorded as the ID of the first app install.

**Method signature**

```c++
std::string GetAppsFlyerUID()
```

**Usage**:

```c++
AppsflyerSteamModule()->GetAppsFlyerUID();
```

### To_utf8

This method receives a reference of a `std::wstring` and returns UTF-8 encoded `std::string`

**Method signature**

```c++
std::string to_utf8(std::wstring& wide_string);
```
**Usage**:
```c++
std::wstring ws = L"車B1234 こんにちは";
std::wstring ws2 = L"新人邀约购物日";
custom_event_parameters = { 
    {"goodsName", AppsflyerLauncherModule()->to_utf8(ws)}, 
    {"goodsName2", AppsflyerLauncherModule()->to_utf8(ws2)} 
};
```


### IsInstallOlderThanDate

This method receives a date string and returns true if the game folder modification date is older than the date string. The date string format is: "2023-January-01 23:12:34"

**Method signature**

```c++
bool IsInstallOlderThanDate(std::string datestring)
```

**Arguments**:

- `std::string datestring`: Date string in `yyyy-mm-ddThh:mm:ss+hh:mm` format.

**Usage**:
<div id="skipFirstExample"></div>

```c++
// the modification date in this example is "2023-January-23 08:30:00"

// will return false
bool dateBefore = AppsflyerSteamModule()->IsInstallOlderThanDate("2023-January-01 23:12:34");

// will return true
bool dateAfter = AppsflyerSteamModule()->IsInstallOlderThanDate("2023-April-10 23:12:34");

// example usage with skipFirst -
// skipping if the install date is NOT older than the given date
bool isInstallOlderThanDate = AppsflyerSteamModule()->IsInstallOlderThanDate("2023-January-10 23:12:34");
AppsflyerSteamModule()->Start(!isInstallOlderThanDate);
```

## Running the sample app

1. Install [Visual Studio](https://visualstudio.microsoft.com/).
2. Open the solution `../appsflyer-steam-sample-app/steam-sample-app/steamworksexample/SteamworksExample.sln`.
3. Open the `Source Files/Main.cpp` file.
4. On line 244, replace `DEV_KEY` and `STEAM_APP_ID` with your [app details](#app-details).
5. Run the app by clicking **Play** on the top toolbar (Local Windows Debugger). Make sure that the mode is set to Debug.
   ![Visual Studio Toolbar Image](https://files.readme.io/f25ef88-vs-run.png "Visual Studio Toolbar Image")
6. After 24 hours, the dashboard updates and shows organic and non-organic installs and in-app events.

## Implementing AppsFlyer in your Steam game

### Setup

1. Copy the files from the `appsflyer-module` folder into your C++ project under **Header Files** > **AppsFlyer**.
2. Import the Module:

```c++
#include "AppsflyerSteamModule.h"
```

3. Import `nlohmann-json`.

```c++
#include <nlohmann/json.hpp>
using json = nlohmann::json;
```

4. [Initialize](#init) the AppsFlyer integration and call [start](#start).
5. Report [in-app events](#logevent).

## Deleting Steam cloud saves (resetting the attribution)

1. [Disable Steam cloud](https://help.steampowered.com/en/faqs/view/68D2-35AB-09A9-7678#enabling).
2. [Delete the local files](https://help.steampowered.com/en/faqs/view/68D2-35AB-09A9-7678#where).
3. Remove the registry data from `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`. The Registry keys are `AF_counter_[APPID]` and `AF_uuid_[APPID]`.