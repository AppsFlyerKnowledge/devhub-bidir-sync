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
[block:html]
{
  "html": "\n<table style=\"width: 100%;\">\n\t<tbody>\n\t\t<tr>\n\t\t\t<td style=\"width: 25.0125%;\"><strong>Platform</strong></td>\n\t\t\t<td style=\"width: 25.0125%;\"><strong>Sub-platform</strong></td>\n\t\t\t<td style=\"width: 24.983%;\">User Agent Example</td>\n\t\t\t<td style=\"width: 24.992%;\">Code example</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td rowspan=\"3\" style=\"width: 25.0125%; vertical-align: middle;\">Steam (Steamworks SDK)</td>\n\t\t\t<td style=\"width: 25.0125%;\">Vanilla (C++)</td>\n\t\t\t<td style=\"width: 24.983%;\">Valve/Steam HTTP Client 1.0 (480)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\">Set automatically by Steam HTTP\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td style=\"width: 25.0125%;\">Unity (C#)</td>\n\t\t\t<td style=\"width: 24.983%;\">\n\n\t\t\t\t<p>Valve/Steam HTTP Client 1.0 ( )</p>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\">Set automatically by Steam HTTP\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td style=\"width: 25.0125%;\">Unreal (C++)</td>\n\t\t\t<td style=\"width: 24.983%;\">Valve/Steam HTTP Client 1.0 (480)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\">Set automatically by Steam HTTP</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td style=\"vertical-align: middle; width: 25.0125%;\">Roku</td>\n\t\t\t<td style=\"width: 25.0125%;\">Roku (BrightScript)</td>\n\t\t\t<td style=\"width: 24.983%;\">\n\n\t\t\t\t<p>Roku/DVP-11.5 (11.5.0.4312-C2)</p>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\">Set automatically by Roku\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td rowspan=\"3\" style=\"width: 25.0125%; vertical-align: middle;\">Epic</td>\n\t\t\t<td style=\"width: 25.0125%;\">Vanilla (C++)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.983%;\">\n\n\t\t\t\t<p>EpicGamesLaucnher/2.2.0 (Windows 10 &nbsp;10.0.19044 64bit)</p>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\">Set manually</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td style=\"width: 25.0125%;\">Unity (C#)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.983%;\">EpicGamesLaucnher/2.2.0 (Windows 10 &nbsp;10.0.19044 64bit)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\"><a href=\"https://github.com/AppsFlyerSDK/appsflyer-unity-epic-sample-app/blob/0ed42640a2d34033271ed094b5502678aaf8bd46/Assets/Scenes/AppsflyerEpicModule.cs#L142-L163\" rel=\"noopener\" target=\"_blank\">github</a>\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t</tr>\n\t\t<tr>\n\t\t\t<td style=\"width: 25.0125%;\">Unreal (C++)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.983%;\">EpicGamesLaucnher/2.2.0 (Windows 10 &nbsp;10.0.19044 64bit)\n\t\t\t\t<br>\n\t\t\t</td>\n\t\t\t<td style=\"width: 24.992%;\"><a href=\"https://github.com/AppsFlyerSDK/appsflyer-unreal-epic-sample-app/blob/45230e91a20615e4691878af7271fced5ad69034/AppsflyerEpicIntegrationFiles/AppsflyerEpicModule/AppsflyerModule.cpp#L57-L61\" rel=\"noopener noreferrer\" target=\"_blank\">github</a></td>\n\t\t</tr>\n\t</tbody>\n</table>"
}
[/block]



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