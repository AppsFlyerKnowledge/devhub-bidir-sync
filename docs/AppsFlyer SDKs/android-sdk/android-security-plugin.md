---
title: AppsFlyer Security Plugin
excerpt: >-
  A Gradle plugin for creating, downloading, and integrating the AppsFlyer
  Security SDK
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# AppsFlyer Security Plugin

[![Maven Central](https://img.shields.io/nexus/r/com.appsflyer/af-security-plugin?server=https%3A%2F%2Foss.sonatype.org)](https://repo1.maven.org/maven2/com/appsflyer/af-security-plugin/)

* [Overview](#overview)
* [Minimum Versions](#minimum-versions)
* [Installation](#installation)
    + [Step 1](#step-1)
    + [Step 2](#step-2)
    + [Step 3](#step-3)
    + [Legacy Install](#legacy-installation)
* [Basic Configuration](#basic-configuration)
* [Configuration Options](#configuration-options)
* [Advanced Configuration](#advanced-configuration)
    + [Flavor Configuration Handling and Priorities](#flavor-configuration-handling-and-priorities)
    + [Advanced Flavor Configuration](#advanced-flavor-configuration)
    + [Advanced Configuration Options](#advanced-configuration-options)

<a name="overview"></a>
## Overview

A Gradle plugin for Android applications module that automates the process of creating, downloading, and integrating the AppsFlyer Security SDK for every build and will keep you updated with the latest security updates.

It's crucial to note that this plugin only works within an application module where the `com.android.application` plugin is applied.


<a name="minumum-versions"></a>
## Minimum Versions

Ensure your project meets the following minimum versions for compatibility with the AppsFlyer Security Plugin:

- [AppsFlyer Android SDK](https://dev.appsflyer.com/hc/docs/android-sdk): Version 6.12.5
- [Android Gradle Plugin (AGP)](https://developer.android.com/build): Version 7.0.0
- [Gradle](https://gradle.org/): Version 7.0

<a name="installation"></a>
## Installation

**Using the [plugins DSL](https://docs.gradle.org/current/userguide/plugins.html#sec:plugins_block):**

### Step 1
The plugin is hosted on Maven Central.
You first need to add the Maven Central as a repository in your `settings.gradle` or `settings.gradle.kts` file:
```groovy
pluginManagement {
    repositories {
        mavenCentral()
        // other repositories...
    }
}
```
```kotlin
pluginManagement {
    repositories {
      mavenCentral()
        // other repositories...
    }
}
```

### Step 2
Declare the plugin in the `plugins` block of your root-level (`project-level`) Gradle file.
Usually `<project>/build.gradle` or `<project>/<app-module>/build.gradle.kts` file.

```groovy
plugins {
    id 'com.android.application' version "$AGP_VERSION" apply false
    id 'com.appsflyer.security' version "$APPSFLYER_SECURITY_PLUGIN_VERSION" apply false
    // other plugins ...
}
```
```kotlin
plugins {
    id("com.android.application") version "$AGP_VERSION" apply false
    id("com.appsflyer.security") version "$APPSFLYER_SECURITY_PLUGIN_VERSION" apply false
    // other plugins ...
}
```


### Step 3

Apply the plugin in your app-level Gradle file.
Usually `<project>/<app-module>/build.gradle` or `<project>/<app-module>/build.gradle.kts` file

```groovy
plugins {
    id 'com.android.application'
    id 'com.appsflyer.security'
    // other plugins...
}
```
```kotlin
plugins {
    id("com.android.application")
    id("com.appsflyer.security")
    // other plugins...
}
```

<a name="legacy-installation"></a>
### Legacy Installation
**Using [legacy plugin application](https://docs.gradle.org/current/userguide/plugins.html#sec:old_plugin_application):**
```groovy
buildscript {
  repositories {
    maventCentral()
  }
  dependencies {
    classpath "com.appsflyer:af-security-plugin:$APPSFLYER_SECURITY_PLUGIN_VERSION"
  }
}

apply plugin: "com.appsflyer.security"
```
```kotlin
buildscript {
  repositories {
    maventCentral()
  }
  dependencies {
    classpath("com.appsflyer:af-security-plugin:$APPSFLYER_SECURITY_PLUGIN_VERSION")
  }
}

apply(plugin = "com.appsflyer.security")
```

**Notes:**
1. Replace `$AGP_VERSION` with the actual version of the Android Gradle Plugin.
2. Replace `$APPSFLYER_SECURITY_PLUGIN_VERSION` with the actual version of the AppsFlyer Security Plugin.


<a name="basic-configuration"></a>
## Basic Configuration
Configure the AppsFlyer security plugin in your app-level Gradle file. Usually `<project>/<app-module>/build.gradle` or `<project>/<app-module>/build.gradle.kts` file.
</br>Ensure to keep sensitive information such as `authToken` and `certificateHashes` in a secure place and avoid including these directly as plain text in the configuration block.

```groovy
appsFlyerSecurityPlugin {
    defaultConfig {
        certificateHashes = ['defaultHash1', 'defaultHash2']
        authToken = 'defaultAuthToken'
    }
}
```
```kotlin
appsFlyerSecurityPlugin {
    defaultConfig {
        certificateHashes.set(listOf("defaultHash1", "defaultHash2"))
        authToken.set("defaultAuthToken")
    }
}
```

<a name="options"></a>

## Configuration Options

Each configuration block (`defaultConfig` or a specific `flavorConfig`) may include:

| Option              | Type           | Description                                                                                                                                                                                                                                                                                                                                                   | Required | Important Note                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |  
|---------------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
| `certificateHashes` | `List<String>` | This field specifies a list of the application signing keys that will be used to validate the Application Integrity. This is mandatory and requires you to supply the certificate hash for your signing key manually as AppsFlyer cannot automatically determine the signing key used to sign your application. Only SHA-256 hashes are supported.            | Yes      | **1.** When using app signing by [Google Play](https://developer.android.com/studio/publish/app-signing#google-play-app-signing), Google manages and protects your app's signing key for you and signs your APKs on your behalf. In this case it is required that you provide the certificate hash for the signing key **used by Google** using this option. This is **always** the case when you distribute Android app bundles. </br>**2.** If `certificateHashes` is not specified or if the provided value doesn't match the build, the validation will fail and the traffic will be marked as fraud. |  
| `authToken`         | `String`       | This is an API V2 token required for plugin authentication. You can retrieve the token in your AppsFlyer [dashboard](https://support.appsflyer.com/hc/en-us/articles/360004562377).                                                                                                                                                                           | Yes      | If `authToken` is not specified or is incorrect, the plugin cannot connect to the AppsFlyer server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |  

In the event of any missing required values (`certificateHashes` and `authToken`), the plugin will not work as expected. The absence of `certificateHashes` will result in validation failure and potential detection of traffic as fraud, whereas the absence of a valid `authToken` will prevent the plugin from functioning properly.

<a name="advanced-configuration"></a>
## Advanced Configuration

<a name="flavor-configuration"></a>
### Flavor Configuration Handling and Priorities

The plugin allows you to define specific configurations for certain flavors or build types of your app. The plugin resolves configurations in the following priority order:

1. **Complete Build Variant:** This configuration block is associated with a complete build variant. A complete build variant is a combination of all flavor dimensions and the build type. For example, given the flavor dimensions "tier" (with values "free", "paid") and "server" (with values "dev", "prod"), a complete build variant could be "freeDevDebug" or "paidProdRelease".

2. **Combined Product Flavor:** If there is no configuration specified for the complete build variant, the plugin will attempt to find a configuration that matches the combined product flavor. Product flavor configuration blocks are named after the combined flavor dimensions. For example, if you're building a variant with a combined flavor of 'paid' and 'prod', the plugin would look for a configuration block named 'paidProd'.

3. **Build Type (Debug/Release):** If no configurations for the complete build variant or combined product flavors are found, the plugin will then look for a configuration block named after the build type. This will be applied to all flavors that are built with this type.

4. **Default:** If none of the above match, the plugin defaults to the `defaultConfig`, applying it to all variants.


<a name="flavor-configuration"></a>
### Advanced Flavor Configuration

```groovy
appsFlyerSecurityPlugin {
    defaultConfig {
        certificateHashes = ['defaultHash1', 'defaultHash2']
        authToken = 'defaultAuthToken'
    }
    flavorConfigs {
        paidProdRelease {
            certificateHashes = ['paidProdReleaseHash1', 'paidProdReleaseHash2']
            authToken = 'paidProdReleaseAuthToken'
            channel = 'paidProdReleaseChannel'
        }
        paidDev {
            certificateHashes = ['paidDevHash1', 'paidDevHash2']
            authToken = 'paidDevAuthToken'
            channel = 'paidDevChannel'
        }
    }
}
```
```kotlin
appsFlyerSecurityPlugin {
    defaultConfig {
        certificateHashes.set(listOf("defaultHash1", "defaultHash2"))
        authToken.set("defaultAuthToken")
    }
    flavorConfig("paidProdRelease") {
        certificateHashes.set(listOf("paidProdReleaseHash1", "paidProdReleaseHash2"))
        authToken.set("paidProdReleaseAuthToken")
        channel.set("paidProdReleaseChannel")
    }
    flavorConfig("paidDev") {
        certificateHashes.set(listOf("paidDevHash1", "paidDevHash2"))
        authToken.set("paidDevAuthToken")
        channel.set("paidDevChannel")
    }
}
```

<a name="advanced-configuration-options"></a>
### Advanced Configuration Options


| Option              | Type           | Description                                                                                                                                                                                                                                                                                                                                                   | Required | Important Note                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |  
|---------------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
| `channel`           | `String`       | This channel specifies the unique store of your Android App when it is added via the Android Out-of-Store APK option. This combination of Android package name and channel uniquely identifies each AppsFlyer dashboard. This option is relevant only when using the dashboard per store option. If not specified, the default value is an empty string `""`. | No       | If the channel value is either blank or is one among "googleplay", "playstore", or "googleplaystore", it is ignored and the returned value will be an empty string, `""`.                                                                                                                                                                                                                                                                                                                                                                                                                                 |