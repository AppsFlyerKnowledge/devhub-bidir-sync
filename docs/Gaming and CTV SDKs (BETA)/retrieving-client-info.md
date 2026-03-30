---
title: Retrieving Client Info
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
## Unity

### User Agent

#### Epic

```
// set the request body
var uwr = new UnityWebRequest(url, "POST");
byte[] jsonToSend = new System.Text.UTF8Encoding().GetBytes(json);
uwr.uploadHandler = (UploadHandler)new UploadHandlerRaw(jsonToSend);
uwr.downloadHandler = (DownloadHandler)new DownloadHandlerBuffer();

var pack = Client.List();
while (!pack.IsCompleted)
  yield return null;
var eosPack = pack.Result.FirstOrDefault(q => q.name == "com.playeveryware.eos");

// set the request content type
uwr.SetRequestHeader("Content-Type", "application/json");
// set the authorization
uwr.SetRequestHeader("Authorization", auth);
uwr.SetRequestHeader(
  "user-agent",
  "EpicGamesLaucnher/"
  + eosPack.version
  + " ("
  3+ SystemInfo.operatingSystem.Replace("(", "").Replace(")", "")
  + ")"
);
```



#### Steam

> 📘 User Agent is sent automatically by the Steam HTTP Client

### Device Model

```
device_model = SystemInfo.deviceModel
```



### OS Version

```
string device_os_ver = SystemInfo.operatingSystem;

// formating for API Restrictions
if (device_os_ver.IndexOf(" (") > -1)
  device_os_ver = device_os_ver.Replace(" (", "");
if (device_os_ver.IndexOf("(") > -1)
  device_os_ver = device_os_ver.Replace("(", "");
if (device_os_ver.IndexOf(")") > -1)
  device_os_ver = device_os_ver.Replace(")", "");
if (device_os_ver.IndexOf("%20") > -1)
  device_os_ver = device_os_ver.Replace("%20", "-");
if (device_os_ver.IndexOf(" ") > -1)
  device_os_ver = device_os_ver.Replace(" ", "-");
device_os_ver = Regex.Replace(device_os_ver, "[^0-9.+-]", "");
if (device_os_ver.IndexOf("-") == 0)
  device_os_ver = device_os_ver.Substring(1, device_os_ver.Length - 1);
if (device_os_ver.Length > 23)
  device_os_ver = device_os_ver.Substring(0, 23);
```



## Unreal

### User Agent

#### Epic

```
FString OsVersion, OsSubVersion;
FPlatformMisc::GetOSVersions(OsVersion, OsSubVersion);
FString userAgent = "EpicGamesLaucnher/2.2.0 (" + OsVersion + ")";
pRequest->SetHeader(TEXT("User-Agent"), *userAgent);
```



#### Steam

> 📘 User Agent is sent automatically by the Steam HTTP Client

### Device Model

TBA

### OS Version

```
FString OsVersion, OsSubVersion;
FPlatformMisc::GetOSVersions(OsVersion, OsSubVersion);
```