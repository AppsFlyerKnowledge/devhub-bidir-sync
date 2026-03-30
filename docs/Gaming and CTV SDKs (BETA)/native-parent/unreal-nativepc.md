---
title: Native PC Unreal
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
> [GitHub](https://github.com/AppsFlyerSDK/appsflyer-native-pc-unreal-sample-app)

## AppsFlyer Native PC Unreal SDK integration

AppsFlyer empowers gaming marketers to make better decisions by providing powerful tools to perform cross-platform attribution.

Game attribution requires the game to integrate the AppsFlyer SDK that records first opens, consecutive sessions, and in-app events. For example, purchase events.
We recommend you use this sample app as a reference for integrating the AppsFlyer SDK into your Unreal game. 

**Note**: The sample code that follows is currently only supported in a Windows environment.

### Prerequisites

- Unreal Engine 4.2x.

<hr/>

## AppsflyerPCModule - Interface

`AppsflyerPCModule.h`, included in the `appsflyer-native-pc-unreal-sample-app/AppsflyerPCModule` folder, contains the required code and logic to connect to AppsFlyer servers and report events.

### Init

This method receives your API key and app ID and initializes the AppsFlyer Module.

**Method signature**

```c++
void Init(const char* devkey, const char* appID)
```

<span id="app-details">**Arguments**:</span>

- `DEV_KEY`: Get from the marketer or [AppsFlyer HQ](https://support.appsflyer.com/hc/en-us/articles/211719806-App-settings-#general-app-settings).
- `APP_ID`: Your Appsflyer app id (excluding the `nativepc-` prefix).


**Usage**:

```c++
AppsflyerPCModule()->Init(<< DEV_KEY >>, << APP_ID >>);
```

### Start

This method sends first open/session requests to AppsFlyer.

**Method signature**

```c++
void Start(bool skipFirst = false)
```

**Arguments**

- `bool skipFirst`: Determines whether or not to skip first open events and send session events. The value is false by default. If true , first open events are skipped and session events are sent. [See example](#skipFirstExample)

**Usage**:

```c++
// without the flag
AppsflyerPCModule()->Start();

// with the flag
bool skipFirst = [SOME_CONDITION];
AppsflyerPCModule()->Start(skipFirst);
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
AppsflyerPCModule()->Start();
// ...
// Stopping the SDK, preventing further communication with AppsFlyer
AppsflyerPCModule()->Stop();
```

### LogEvent

This method receives an event name and JSON object and sends in-app events to AppsFlyer.

**Method signature**

```c++
void LogEvent(std::string event_name, std::string event_values, std::string custom_event_values = "")
```

**Arguments**

- `std::string event_name`-
- `std::string event_parameters`: dictionary object which contains the [predefined event parameters](https://dev.appsflyer.com/hc/docs/ctv-log-event-event-parameters).
- `std::string event_custom_parameters` (non-mandatory): dictionary object which contains the any custom event parameters. For non-English values, please use [UTF-8 encoding](#to_utf8).

**Usage**:

```c++
// Setting the event parameters json string and event name
std::string event_name = "af_purchase";
std::string event_parameters = "{\"af_currency\":\"USD\",\"af_revenue\":24.12}";
// Send the InApp event request
AppsflyerPCModule()->LogEvent(event_name, event_parameters);

// Set non-English values for testing UTF-8 support
std::wstring ws = L"車B1234 こんにちは";
std::wstring ws2 = L"新人邀约购物日";
std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerPCModule()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerPCModule()->to_utf8(ws2) + "\"}";
// Send inapp event with custom params
AppsflyerPCModule()->LogEvent(event_name, event_parameters, event_custom_parameters);
```

### GetAppsFlyerUID

Get AppsFlyer's unique device ID. The SDK generates an AppsFlyer unique device ID upon app installation. When the SDK is started, this ID is recorded as the ID of the first app install.

**Method signature**

```c++
std::string GetAppsFlyerUID()
```

**Usage**:

```c++
AppsflyerPCModule()->GetAppsFlyerUID();
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
AppsflyerPCModule()->Init(DEV_KEY, APP_ID);
AppsflyerPCModule()->SetCustomerUserId("Test-18-9-23");
AppsflyerPCModule()->Start();
```


### To_utf8

This method receives a reference of a `std::wstring` and returns UTF-8 encoded `std::string`

**Method signature**

```c++
std::string to_utf8(std::wstring& wide_string);
```
**Usage**:
```c++
// Set non-English values for testing UTF-8 support
std::wstring ws = L"車B1234 こんにちは";
std::wstring ws2 = L"新人邀约购物日";
std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerPCModule()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerPCModule()->to_utf8(ws2) + "\"}";
```

### IsInstallOlderThanDate

This method receives a date string and returns true if the game folder modification date is older than the date string. The date string format is: "2023-January-01 23:12:34"

**Method signature**

```c++
bool IsInstallOlderThanDate(std::string datestring)
```

**Arguments**:

- `std::string datestring`: Date string in `yyyy-MMMM-dd hh:mm:ss` format.

**Usage**:
<div id="skipFirstExample"></div>

```c++
// the modification date in this example is "2023-January-23 08:30:00"

// will return false
bool dateBefore = AppsflyerPCModule()->IsInstallOlderThanDate("2023-January-01 23:12:34");

// will return true
bool dateAfter = AppsflyerPCModule()->IsInstallOlderThanDate("2023-April-10 23:12:34");

// example usage with skipFirst:
bool isInstallOlderThanDate = AppsflyerPCModule()->IsInstallOlderThanDate("2023-April-10 23:12:34");
AppsflyerPCModule()->Start(isInstallOlderThanDate);
```

## Running the sample app

1. Open the UE4 engine.
2. Choose **New Project** > **Games** > **First Person**.
3. Choose C++ (instead of Blueprints).
4. Name the project `AppsFlyerSample` and click **Create project**.
5. Follow the [instructions to implement AppsFlyer in your game](#implementing-appsflyer-in-your-unreal-game).
6. Launch the sample app from the UE4 engine editor.
7. After 24 hours, the dashboard updates and shows organic and non-organic installs and in-app events.

## **Implementing AppsFlyer in your Unreal game**

### Setup

1. Open the project in your preferred C++ editor and in the `[YOUR-APP-NAME].Build.cs` file, add `OpenSSL` to your dependencies and `HTTP` as a private dependency:

```c#
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "HeadMountedDisplay", "OpenSSL" });
PrivateDependencyModuleNames.Add("HTTP");
```

2. In your Unreal Project files, under the `Source/[YOUR-APP-NAME]` directory, create a new directory named `AppsflyerPCModule`.
3. Copy the following files from `appsflyer-native-pc-unreal-sample-app/AppsflyerPCModule` to the new folder:

   - AppsflyerPCModule.cpp
   - AppsflyerPCModule.cpp
   - AppsflyerPCModule.h
   - DeviceID.h
   - RequestData.h

4. Generate project files in order to add OpenSSL. [Learn more](https://forums.unrealengine.com/t/how-to-use-included-openssl/670971/2)
5. In the `GameMode.h` file, add the `StartPlay()` function:

```c++
UCLASS(minimalapi)
class AAppsFlyerSampleGameMode : public AGameModeBase
{
 GENERATED_BODY()

public:
 AAppsFlyerSampleGameMode();
 virtual void StartPlay() override;
};

```

6. Open the `Source/[YOUR-APP-NAME]/AppsFlyerSampleGameMode.cpp` file and add the following include to your GameMode.cpp file.

```c++
#include "AppsflyerPCModule/AppsflyerPCModule.h"
using Super = AGameModeBase;
```

7. Add the following function, making sure to replace `DEV_KEY` and `APP_ID` in the [`init`](#init) function with your [app details](#app-details), and report [in-app events](#logevent)

```c++
void AAppsFlyerSampleGameMode::StartPlay()
{
   Super::StartPlay();
   // af module init
   AppsflyerPCModule()->Init(<< DevKey >>, << AppID >>);

   // af send firstopen/session
   AppsflyerPCModule()->Start();

   // set event name
   std::string event_name = "af_purchase";
   // set json string
   std::string event_parameters = "{\"af_currency\":\"USD\",\"af_revenue\":24.12}";
   // af send inapp event
   AppsflyerPCModule()->LogEvent(event_name, event_parameters);
   if (AppsflyerPCModule()->IsInstallOlderThanDate("2025-January-01 23:12:34")) {
      UE_LOG(LogTemp, Warning, TEXT("2025-January-01 23:12:34: true"));
   }
   else {
      UE_LOG(LogTemp, Warning, TEXT("2025-January-01 23:12:34: false"));
   }
   if (AppsflyerPCModule()->IsInstallOlderThanDate("2023-January-01 23:12:34")) {
      UE_LOG(LogTemp, Warning, TEXT("2023-January-01 23:12:34: true"));
   }
   else {
      UE_LOG(LogTemp, Warning, TEXT("2023-January-01 23:12:34: false"));
   }

   // set non-English values for testing UTF-8 support 
   std::wstring ws = L"車B1234 こんにちは";
   std::wstring ws2 = L"新人邀约购物日";
   std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerPCModule()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerPCModule()->to_utf8(ws2) + "\"}";
   // af send inapp event with custom params
   AppsflyerPCModule()->LogEvent(event_name, event_parameters, event_custom_parameters);

   // stop the SDK
   AppsflyerPCModule()->Stop();
}
```

## Resetting the attribtion

Delete the `appsflyer_info` file:

![Delete the](https://files.readme.io/d43d8ce-image.png)