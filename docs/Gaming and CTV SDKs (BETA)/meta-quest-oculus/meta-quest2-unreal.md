---
title: Meta Quest 2 Unreal
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
> [GitHub](https://github.com/AppsFlyerSDK/appsflyer-meta-quest2-unreal-sample-app)

## AppsFlyer Meta Quest 2 Unreal SDK integration

Integrate your Meta Quest Unreal app or game with AppsFlyer to measure the performance of campaigns marketing these apps.

Game attribution and user measurement require the game to integrate an AppsFlyer SDK that records first opens, sessions, and in-app events. For example, purchase events.

**Note**: The sample code that follows is currently only supported in a Windows environment.

### Prerequisites

- Unreal Engine 4.2x.
- Follow the [Quest Integration guide for Quest 2 and Unreal Engine](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/XRDevelopment/VR/VRPlatforms/Oculus/OculusQuest/).
- Follow the [Quest 2 & Unreal 4.27](https://stackoverflow.com/a/70818913) integration guide.

<hr/>

## AppsflyerQuest2Module - Interface

`AppsflyerQuest2Module.h`, included in the `appsflyer-meta-quest2-unreal-sample-app/Quest2_Sample/AppsflyerQuest2Module` folder, contains the required code and logic to connect to AppsFlyer servers and report events.

### Init

This method receives your API key and app ID and initializes the AppsFlyer Module.

**Method signature**

```c++
void Init(const char* devkey, const char* appID)
```

**Usage**:

```c++
AppsflyerQuest2Module()->Init(<< DEV_KEY >>, << QUEST_APP_ID >>);
```

<span id="app-details">**Arguments**:</span>

- `DEV_KEY`: Get from the marketer or [AppsFlyer HQ](https://support.appsflyer.com/hc/en-us/articles/211719806-App-settings-#general-app-settings).
- `QUEST_APP_ID`: Your Quest Store app ID (For Quest 2, it's the number in the store URL - for example: `https://www.oculus.com/experiences/quest/XXXXXXXXXXXXXXXX/`).

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
AppsflyerQuest2Module()->Start();

// with the flag
bool skipFirst = [SOME_CONDITION];
AppsflyerQuest2Module()->Start(skipFirst);
```

### Stop

This method stops the SDK from functioning and communicating with AppsFlyer servers. It's used when implementing user opt-in/opt-out.

**Method signature**

```c#
void Stop()
```

**Usage**:

```c#
// Starting the SDK
AppsflyerQuest2Module()->Start();
// ...
// Stopping the SDK, preventing further communication with AppsFlyer
AppsflyerQuest2Module()->Stop();
```

### LogEvent

This method receives an event name and JSON object and sends in-app events to AppsFlyer.

**Method signature**

```c++
void LogEvent(std::string event_name, std::string event_values, std::string custom_event_values = "")
```

**Arguments**

- `std::string event_name`: The name of the event.
- `std::string event_parameters`: String which contains the [predefined event parameters](https://dev.appsflyer.com/hc/docs/ctv-log-event-event-parameters).
- `std::string event_custom_parameters` [Optional]: String which contains the any custom event parameters. For non-English values, please use [UTF-8 encoding](#to_utf8).

**Usage**:

```c++
// Setting the event parameters json string and event name
std::string event_name = "af_purchase";
std::string event_parameters = "{\"af_currency\":\"USD\",\"af_revenue\":24.12}";
// Send the InApp event request
AppsflyerQuest2Module()->LogEvent(event_name, event_parameters);

// Set non-English values for testing UTF-8 support
std::wstring ws = L"車B1234 こんにちは";
std::wstring ws2 = L"新人邀约购物日";
std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerQuest2Module()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerQuest2Module()->to_utf8(ws2) + "\"}";
// Send inapp event with custom params
AppsflyerQuest2Module()->LogEvent(event_name, event_parameters, event_custom_parameters);
```

### GetAppsFlyerUID

Get the AppsFlyer unique device ID. The SDK generates an AppsFlyer unique device ID upon app installation. When the SDK is started, this ID is recorded as the ID of the first app install.

**Method signature**

```c++
std::string GetAppsFlyerUID()
```

**Usage**:

```c++
AppsflyerQuest2Module()->GetAppsFlyerUID();
```

### SetCustomerUserId

This method sets a customer ID that enables you to cross-reference your unique ID with the AppsFlyer unique ID and other device IDs. **Note**: You can only use this method before calling `Start()`.
The customer ID is available in raw data reports and in the postbacks sent via API.

**Method signature**

```c++
void SetCustomerUserId(std::string cuid)
```

**Arguments**:

- `std::string cuid`: Custom user id.

**Usage**:

```c++
AppsflyerQuest2Module()->Init(DEV_KEY, APP_ID);
AppsflyerQuest2Module()->SetCustomerUserId("Test-18-9-23");
AppsflyerQuest2Module()->Start();
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
std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerQuest2Module()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerQuest2Module()->to_utf8(ws2) + "\"}";
```

### IsInstallOlderThanDate

This method receives a date string and returns true if the game folder modification date is older than the date string. The date string format is: "2023-January-01 23:12:34".

**Method signature**

```c++
bool IsInstallOlderThanDate(std::string datestring)
```

**Arguments**:

- `std::string datestring`: Date string in `yyyy-mm-ddThh:mm:ss+hh:mm` format.

**Usage**:
<div id="skipFirstExample"></div>

```c++
// the modification date in this example is "2023-July-23 08:30:00"

// will return false
bool dateBefore = AppsflyerQuest2Module()->IsInstallOlderThanDate("2023-January-01 23:12:34");

// will return true
bool dateAfter = AppsflyerQuest2Module()->IsInstallOlderThanDate("2023-September-10 23:12:34");

// example usage with skipFirst:
bool isInstallOlderThanDate = AppsflyerLauncherModule()->IsInstallOlderThanDate("2023-April-10 23:12:34");
AppsflyerLauncherModule()->Start(isInstallOlderThanDate);
```

## Running the sample app

1. Open the UE4 engine.
2. Choose **New Project** > **Games** > **Virtual Reality**.
3. Name the project `Quest2_Sample` and click **Create project**.
4. Follow the [instructions to implement AppsFlyer in your game](#implementing-appsflyer-in-your-unreal-game).
5. Launch the sample app from the UE4 engine editor.
6. After 24 hours, the dashboard updates and shows organic and non-organic installs and in-app events.

## **Implementing AppsFlyer in your Unreal game**

### Setup

1. Open the project in your preferred C++ editor and in the `[YOUR-APP-NAME].Build.cs` file, add `OpenSSL` to your dependencies and `HTTP` as a private dependency:
(example can be found in `/Quest2_Sample/Quest2_Sample.Build`)

```c#
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "HeadMountedDisplay", "OpenSSL" });
PrivateDependencyModuleNames.Add("HTTP");
```

2. In your Unreal Project files, under the `Source/[YOUR-APP-NAME]` directory, create a new directory named `AppsflyerQuest2Module`.
3. Copy the following files from `appsflyer-meta-quest2-unreal-sample-app/AppsflyerQuest2Module` to the new folder:

   - AppsflyerQuest2Module.cpp
   - AppsflyerQuest2Module.cpp
   - AppsflyerQuest2Module.h
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

6. Create a C++ Actor Component, attach it to a object of your choice, and add the following lines: 
(Alternatively you may use the example component: `Quest2_Sample/Private/AF_ActorComponent.cpp`)

```c++
#include "../AppsflyerQuest2Module/AppsflyerQuest2Module.h"
```

7. Add the following function, making sure to replace `DEV_KEY` and `QUEST_APP_ID` in the [`init`](#init) function with your [app details](#app-details), and report [in-app events](#logevent)

```c++
void UAF_ActorComponent::BeginPlay()
{
	Super::BeginPlay();
	// af module init
	AppsflyerQuest2Module()->Init(<< DEV_KEY >> , << PACKAGE_NAME >> );


	// af send firstopen/session
	AppsflyerQuest2Module()->Start();

	// set event name
	std::string event_name = "af_purchase";
	// set json string
	std::string event_parameters = "{\"af_currency\":\"USD\",\"af_revenue\":24.12}";
	// af send inapp event
	AppsflyerQuest2Module()->LogEvent(event_name, event_parameters);

	// set non-English values for testing UTF-8 support
	std::wstring ws = L"車B1234 こんにちは";
	std::wstring ws2 = L"新人邀约购物日";
	std::string event_custom_parameters = "{\"goodsName\":\"" + AppsflyerQuest2Module()->to_utf8(ws) + "\",\"goodsName2\":\"" + AppsflyerQuest2Module()->to_utf8(ws2) + "\"}";
	// af send inapp event with custom params
	AppsflyerQuest2Module()->LogEvent(event_name, event_parameters, event_custom_parameters);

	// stop the SDK
	AppsflyerQuest2Module()->Stop();
}
```

## Resetting the attribtion

Delete the APK from your Oculus Device and then Re-deploy (a new `AppsFlyerUID` will be created).