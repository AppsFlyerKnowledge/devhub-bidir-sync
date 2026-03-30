---
title: OneLink user invite in Android
description: >-
  Create OneLink custom link for user-invite and copy to clipboard when link is
  ready
hidden: false
recipe:
  color: '#018FF4'
  icon: 🦉
---
```java Application
package com.appsflyer.onelink.appsflyeronelinkbasicapp;

import com.appsflyer.AppsFlyerLib;
import com.appsflyer.deeplink.DeepLink;
import com.appsflyer.deeplink.DeepLinkListener;
import com.appsflyer.deeplink.DeepLinkResult;

import android.app.Application;
import android.util.Log;
import androidx.annotation.NonNull;
import java.util.Map;

public class AppsflyerBasicApp extends Application {
    public static final String LOG_TAG = "AppsFlyerOneLinkSimApp";
    public static final String DL_ATTRS = "dl_attrs";
    Map<String, Object> conversionData = null;

    @Override
    public void onCreate() {
        super.onCreate();
        String afDevKey = AppsFlyerConstants.afDevKey;
        AppsFlyerLib appsflyer = AppsFlyerLib.getInstance();
        // Make sure you remove the following line when building to production
        appsflyer.setDebugLog(true);
        appsflyer.setMinTimeBetweenSessions(0);
        //set the OneLink template id for share invite links
        AppsFlyerLib.getInstance().setAppInviteOneLink("H5hv");

        appsflyer.subscribeForDeepLink(new DeepLinkListener(){
            @Override
            public void onDeepLinking(@NonNull DeepLinkResult deepLinkResult) {
                DeepLinkResult.Status dlStatus = deepLinkResult.getStatus();
                if (dlStatus == DeepLinkResult.Status.FOUND) {
                    Log.d(LOG_TAG, "Deep link found");
                } else if (dlStatus == DeepLinkResult.Status.NOT_FOUND) {
                    Log.d(LOG_TAG, "Deep link not found");
                    return;
                } else {
                    // dlStatus == DeepLinkResult.Status.ERROR
                    DeepLinkResult.Error dlError = deepLinkResult.getError();
                    Log.d(LOG_TAG, "There was an error getting Deep Link data: " + dlError.toString());
                    return;
                }
                DeepLink deepLinkObj = deepLinkResult.getDeepLink();
                try {
                    Log.d(LOG_TAG, "The DeepLink data is: " + deepLinkObj.toString());
                } catch (Exception e) {
                    Log.d(LOG_TAG, "DeepLink data came back null");
                    return;
                }
                // An example for using is_deferred
                if (deepLinkObj.isDeferred()) {
                    Log.d(LOG_TAG, "This is a deferred deep link");
                } else {
                    Log.d(LOG_TAG, "This is a direct deep link");
                }
                // An example for getting deep_link_value
                String fruitName = "";
                try {
                    fruitName = deepLinkObj.getDeepLinkValue();
                    
                    String referrerId = "";
                    JSONObject dlData = deepLinkObj.getClickEvent();
                    if (dlData.has("deep_link_sub2")){
                        referrerId = deepLinkObj.getStringValue("deep_link_sub2");
                        Log.d(LOG_TAG, "The referrerID is: " + referrerId);
                    }  else {
                        Log.d(LOG_TAG, "deep_link_sub2/Referrer ID not found");
                    }
                  
                    if (fruitName == null){
                        Log.d(LOG_TAG, "Deeplink value returned null");
                        return;
                    }
                    Log.d(LOG_TAG, "The DeepLink will route to: " + fruitName);
                } catch (Exception e) {
                    Log.d(LOG_TAG, "Custom param fruit_name was not found in DeepLink data");
                    return;
                }
            }
        });
  
        appsflyer.init(afDevKey, null, this);
        appsflyer.start(this);
    }
    
}

```

```java Activity
package com.appsflyer.onelink.appsflyeronelinkbasicapp;

import android.content.ClipData;
import android.content.ClipboardManager;
import android.content.Intent;
import android.os.Bundle;
import android.text.method.ScrollingMovementMethod;
import android.util.Log;
import android.view.Gravity;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.appsflyer.CreateOneLinkHttpTask;
import com.appsflyer.deeplink.DeepLink;
import com.appsflyer.share.LinkGenerator;
import com.appsflyer.share.ShareInviteHelper;
import com.google.gson.Gson;
import org.json.JSONException;
import org.json.JSONObject;

import static com.appsflyer.onelink.appsflyeronelinkbasicapp.AppsflyerBasicApp.LOG_TAG;

public abstract class FruitActivity extends AppCompatActivity {
    TextView dlAttrs;
    TextView dlTitleText;
    TextView goToConversionDataText;
    String fruitName;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(getLayoutResourceId());
        Button sharedInvitesBtn = (Button)findViewById(R.id.shareinvitesbtn);
        sharedInvitesBtn.setOnClickListener(v -> {
            copyShareInviteLink();
        });
    }

    protected abstract int getLayoutResourceId();

    protected void setStaticAttributes(String fruitName) {
        try {
            String dlParamsId = fruitName.concat("_deeplinkparams");
            String dlTitleId = fruitName.concat("_deeplinktitle");
            String conversionDataBtnId = fruitName.concat("_getconversiondata");
            this.dlAttrs = (TextView)findViewById(getResources().getIdentifier(dlParamsId, "id", getPackageName()));
            this.dlTitleText = (TextView)findViewById(getResources().getIdentifier(dlTitleId, "id", getPackageName()));
            this.goToConversionDataText = (TextView)findViewById(getResources().getIdentifier(conversionDataBtnId, "id", getPackageName()));
            this.fruitName = fruitName;
        }
        catch (Exception e){
            Log.d(LOG_TAG, "Error getting TextViews for " + fruitName + " Activity");
        }
        goToConversionDataText.setOnClickListener(v -> {
            Intent intent = new Intent(getApplicationContext(), ConversionDataActivity.class);
            startActivity(intent);
        });
    }

    protected void showDlData() {
        Intent intent = getIntent();
        Gson json = new Gson();
        DeepLink dlData = json.fromJson(intent.getStringExtra(AppsflyerBasicApp.DL_ATTRS), DeepLink.class);
        if (dlData != null) {
            JSONObject jsonObject;
            try {
                jsonObject = new JSONObject(dlData.toString());
                dlAttrs.setMovementMethod(new ScrollingMovementMethod());
                dlAttrs.setText(jsonObject.toString(4).replaceAll("\\\\", ""));// 4 is num of spaces for indent
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        else{
            dlTitleText.setText("No Deep Linking Happened");
        }
    }
    protected void copyShareInviteLink(){
        LinkGenerator linkGenerator = ShareInviteHelper.generateInviteUrl(getApplicationContext());
        linkGenerator.addParameter("deep_link_value", this.fruitName);
        linkGenerator.addParameter("deep_link_sub1", this.fruitAmountStr);
        linkGenerator.addParameter("deep_link_sub2", "THIS_USER_ID");
        linkGenerator.setCampaign("summer_fruits");
        linkGenerator.setChannel("mobile_share");

        Log.d(LOG_TAG, "Link params:" + linkGenerator.getUserParams().toString());
        CreateOneLinkHttpTask.ResponseListener listener = new CreateOneLinkHttpTask.ResponseListener() {
            @Override
            public void onResponse(String s) {
                Log.d(LOG_TAG, "Share invite link: " + s);
                ///use here the link, e.g. copy to clipboard
                });
            }

            @Override
            public void onResponseError(String s) {
                Log.d(LOG_TAG, "onResponseError called");
            }
        };
        linkGenerator.generateLink(getApplicationContext(), listener);
    }
}

```

# Set OneLink template

<!-- java@27 -->

> In the Application tab

Set the OneLink template for the OneLink user invite.
The template is provided by the marketer

# Import libraries

<!-- java@16,18-19 -->

> In the Activity tab

# Create LinkGenerator object

<!-- java@82 -->

> In the Activity tab

Using [`ShareInviteHelper`] create a  [`LinkGenerator`] instance

[`ShareInviteHelper`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-shareshareinvitehelper
[`LinkGenerator`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-sharelinkgenerator

# Set OneLink custom parameters like `deep_link_value`

<!-- java@83-85 -->

> In the Activity tab

Use [`addParameter()`]

[`addParameter()`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-sharelinkgenerator#addparameter

# Set OneLink attribution parameters

<!-- java@86-87 -->

> In the Activity tab

Use native functions (e.g. [`setCampaign`] and [`setChannel`]) to set OneLink parameters (.e.g `campaign` and `channel`)

[`setCampaign`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-sharelinkgenerator#setcampaign
[`setChannel`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-sharelinkgenerator#setchannel

# Create a listener to receive the user-invite link async

<!-- java@90 -->

> In the Activity tab

Use `ResponseListener` to register the async responses for successful or failed link creation

# Successful link creation async response

<!-- java@91-96 -->

> In the Activity tab

`onResponse` is called when the user-invite is created successfully.
In this example the link is copied to the clipboard

# Failed link creation async response

<!-- java@98-101 -->

> In the Activity tab

`onResponseError` is called when the user-invite failed to create

# Generate link

<!-- java@103 -->

> In the Activity tab

Use `generateLink()`

[`generateLink()`]: https://dev.appsflyer.com/hc/docs/android-sdk-reference-sharelinkgenerator#generatelink