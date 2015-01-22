seamless-Android-SDK(v1.5.1)
=========

Requirements
-----
* Minimum Android SDK 4.x (ICS and above)  
 

## [Installation](#installation-1)  



## How to use  

* Integrating Ads  
  * [Setup](#setup)
  * [Integrating Feed Ads](#feed-ad-integration)
  * [Integrating Banner Ads](#banner-integration)
  * [Integrating Full-Page-Layer Ads](#full-page-layer-ad-integration)
  * [Integrating Video Ads](#video-ad-integration)  

* Customizing Ads  
  * [Feed Ad Customization](#feed-ad-customization-recommended)


Installation
-----
* For Eclipse  

    1. Add .jar file to your project  

    ![Alt Text](/ReadMeAssets/DragnDropJar.png)  


    2. Add Seamless Resources folder to your project as a Library  

    ![Alt Text](/ReadMeAssets/addLibrarystep1.png)  

    ![Alt Text](/ReadMeAssets/addLibrarystep2.png)  


    3. Declare the following permissions to your *AndroidManifest.xml* file.
    ```
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    ```

    4. Declare the following activities to your *AndroidManifest.xml* file.  
    ```
    <activity android:name="com.mobilike.seamless.mopub.mobileads.MoPubActivity"
              android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidActivity"
              android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.mobilike.seamless.mopub.common.MoPubBrowser"
              android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidVideoPlayerActivity"
              android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.facebook.ads.InterstitialAdActivity"
            android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScre             enSize" />
    <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
    ```

* For Android Studio  

  In your build.gradle file,    
    * Add following code to repositories --> maven
    ```
    repositories {
        maven {
            url "http://maven.seamlessapi.com:8081/nexus/content/repositories/releases/"
        }
    }
    ```  
    
    * Add the following code to your dependencies
    ```
    dependencies {
        compile 'com.goseamless:seamless:1.5.1'
    }
    ```


  * Declare the following permissions to your *AndroidManifest.xml* file.
  ```
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
  ```

  * Declare the following activities to your *AndroidManifest.xml* file.  
  ```
  <activity android:name="com.mobilike.seamless.mopub.mobileads.MoPubActivity"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidActivity"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.mobilike.seamless.mopub.common.MoPubBrowser"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidVideoPlayerActivity"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.facebook.ads.InterstitialAdActivity"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScre             enSize" />
  <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
  ```


###
How to use
-------  

### Setup  
  In your application class set your App Token
  ```
  @Override
  public void onCreate()
  {
      super.onCreate();
        
      // In the onCreate method, set your app token and application context
      SeamlessConfig.setAppToken("*** Your AppToken ***", this);
  }
   ``` 

### Feed Ad Integration
* Define the *FeedListener*
```
FeedListener feedListener = new FeedListener() {
    @Override
    public void onAdLoad(ListAdapter adapter, OnItemClickListener onItemClickListener) {
        // If ads loaded succesfully,
        // returns an adapter that contains feed ads
        mListView.setAdapter(adapter);

        // If you have an OnItemClickListener on your ListView
        // and you passed it to FeedManager.Builder
        // you should set it here (optional)
        mListView.setOnItemClickListener(onOtemClickListener);
    }

    @Override
    public void onAdFailed(ListAdapter adapter){
        // If ads fail to load, returns your own adapter
        mListView.setAdapter(adapter);
    }
};
```

* Define the *FeedManager*
```
FeedManager feedManager = new FeedManager.Builder(context)
        // Should specify your current content like "yourapp-yourcontent-adtype"
        // i.e.: "myapp-sports-feed"
        .entity("xxx-- Your Entity --xxx") 
        .adapter(adapter)
        .listener(feedListener)
        .parentView(mListview) // or gridview  
        .onItemClickListener(onItemClickListener) // Optional - if you have an onItemClickListener, pass it here
        .category(AdCategories.Uncategorised) // Select proper category eg: News, Sports etc.
        .build();
```  

**NOTE:  What are entities and category? Are they important?**
- Entity names are used by Seamless to distinguish different views and determine whether it should provide ad or not. For example, you could use different entity names for main view and detail view or include menu names in the entity names.
- Category is used by Mopub to provide more relevant advertisement. Accurate category names will return better ads.

> Make sure that you re-create your convertView
> in your ArrayAdapter, if it's tag not an instance of
> your current ViewHolder
```
if(convertView == null || !(convertView.getTag() instanceof ViewHolder))
{
    // logic..
}
```  
> And don't forget to destroy it!
```
@Override
protected void onDestory() {
    if(feedManager != null) {
        feedManager.destroy();
    }
    super.onDestroy();
}
```  
> Also note that, Seamless SDK currently does not support landscape orientation.
> So aviod to inject ads when the device is in landscape mode.



### Banner Integration
* Add *SeamlessMMAView* to your layout  
```
<com.mobilike.seamless.view.SeamlessMMAView
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:id="@+id/adView"/>
```

* Define *SeamlessMMAView* in your activity
```
SeamlessMMAView mAdView = (SeamlessMMAView) findViewById(R.id.adView);
```

* Define the *BannerManagerListener*  
```
BannerManagerListener bannerManagerListener = new BannerManagerListener() {
    @Override
    public void onBannerLoad(FrameLayout bannerView) {
        mAdView.setVisibility(View.VISIBLE);
        mAdView.removeAllViews();
        mAdView.addView(bannerView);
    }

    @Override
    public void onBannerFailed(FrameLayout bannerView, String error) {
        mAdView.setVisibility(View.GONE);
    }
};
```
* Define the BannerManager  
```
 BannerManager bannerManager = new BannerManager.Builder(context)
        // Should specify your current content like "yourapp-yourcontent-adtype"
        // i.e : "yourapp-sports-banner"
        .entity("xxx-- Your Entity --xxx") 
        .adView("xxx-- YOUR AD SIZE --xxx") // This is OPTIONAL i.e. = "250x150"/ the size of your SeamlessMMAView
        .category(AdCategories.Uncategorised) / Select proper category eg: News, Sports etc.
        .listener(bannerManagerListener)
        .build();
```  

**NOTE:  What are entities and category? Are they important?**
- Entity names are used by Seamless to distinguish different views and determine whether it should provide ad or not. For example, you could use different entity names for main view and detail view or include menu names in the entity names.
- Category is used by Mopub to provide more relevant advertisement. Accurate category names will return better ads.

>Don't forget to destory your adView in *onDestroy()* callback
```
@Override
protected void onDestory() {
    bannerManager.destroyAd();
    super.onDestroy();
}
```  

### Full-Page-Layer Ad Integration
* Define the *InterstitialManagerListener*  
```
InterstitialManagerListener intersititalManagerListener = new InterstitialManagerListener() {
    @Override
    public void onInterstitialLoad(MoPubInterstitial mInterstitial, boolean isReady) {
        if(isReady) {
            mInterstitial.show();
        }
    }
    @Override
    public void onInterstitialFailed(String error) {

    }
};
```
* Define the *InterstitialManager*  
```
InterstitialManager interstitialManager = new InterstitialManager.Builder(context)
    // Should specify your current content like "yourapp-yourcontent-adtype"
    // i.e. : "yourapp-sports-fullpagelayer" 
    .entity("xxx-- Your Entity --xxx") 
    .listener(intersititalManagerListener)
    .category(AdCategories.Uncategorised) / Select proper category eg: News, Sports etc.
    .build();
```  

**NOTE:  What are entities and category? Are they important?**
- Entity names are used by Seamless to distinguish different views and determine whether it should provide ad or not. For example, you could use different entity names for main view and detail view or include menu names in the entity names.
- Category is used by Mopub to provide more relevant advertisement. Accurate category names will return better ads.

>Don't forget to destroy your adView in *onDestroy()* callback
```
@Override
protected void onDestory() {
    interstitialManager.destroyAd();
    super.onDestroy();
}
```  

### Video Ad Integration
Currently, Seamless Android SDK only supports preroll ads.  

* Define the *SeamlessPlayerManagerListener*  
```
SeamlessPlayerManagerListener seamlessPlayerManagerListener = new SeamlessPlayerManagerListener() {

    @Override
    public void onAdPrepared(final AdView adView,final  Map<String, Object> params) {

        VPAIDEventListener onAdLoaded = new VPAIDEventListener() {

            @Override
            public void onEvent(VPAIDEvent event) {

                // Create container View to hold the AdView
                // Remove all views from the container and add the AdView
                adViewLayout.removeAllViews();
                adViewLayout.addView(adView);

                // Then start the video ad
                adView.startAd();
            }
        };

        VPAIDEventListener onAdStopped = new VPAIDEventListener() {
             
            @Override
            public void onEvent(VPAIDEvent event) {
                 // Ad Stopped
            } 
        };

        VPAIDEventListener onAdError = new VPAIDEventListener() {
             
            @Override
            public void onEvent(VPAIDEvent event) {
                 // Error
            } 
        };

        // Add the listeners that you defined above
        adView.addEventListener(VPAIDEvent.AdLoaded, onAdLoaded);
        adView.addEventListener(VPAIDEvent.AdStopped, onAdStopped);
        adView.addEventListener(VPAIDAdErrorEvent.AdError, onAdError);

        // Initialize the ad
        adView.initAd(params);
    }

    @Override
    public void onAdFailed(String error) {
                
    }
    
};
```
* Define the *SeamlessPlayerManager*  
```
SeamlessPlayerManager seamlessPlayerManager = new SeamlessPlayerManager.Builder(context)
    .entity("xxx-- Your Token --xxx") // Should specify your current content like "yourapp-yourcontent-adtype"
    .listener(seamlessPlayerManager Listener)
    .build();
```  

**NOTE:  What are entities and category? Are they important?**
- Entity names are used by Seamless to distinguish different views and determine whether it should provide ad or not. For example, you could use different entity names for main view and detail view or include menu names in the entity names.
- Category is used by Mopub to provide more relevant advertisement. Accurate category names will return better ads.

### Feed Ad Customization (Recommended)
* You can set properties to your *FeedManager* object  

  For MAIA Ads, you can set margins:
  ![Alt Text](/ReadMeAssets/maia_guideline_margins.png)  

  For Display Ads, you can set Top and Bottom margins:
  ![Alt Text](/ReadMeAssets/maia_display_ad_Android.png)  

    > Remember! Your margins (left and right) cannot be larger than
    > 32dp; that makes the container smaller than the MAIA Card  

    ```  
    feedManager.setMargins(12,12,24,24); //(Bottom, Top, Left, Right)
    feedManager.setDisplayAdTopMargin(12);
    feedManager.setDisplayAdBottom.Margin(12);
    ```  

   You can set your background colors and shapes by passing your XML files.  
   ![Alt Text](/ReadMeAssets/maia_guideline_backgroundproperties.png)  
    ```
    // You can set Drawables also, just pass the ID's to these methods..
    feedManager.setContainerBackground(R.drawable.bg_container);
    feedManager.setMaiaAdHeaderBackground(R.drawable.bg_maia_ad_header); // Also you can add strokes and corners here
    feedManager.setMaiaAdFooterBackground(R.drawable.bg_maia_ad_footer); // Avoid adding strokes
    feedManager.setMaiaCTAButtonBackground(R.drawable.bg_maia_cta_button);
    ```  

   For the text properties you have these options:
  ![Alt Text](/ReadMeAssets/maia_guideline_textproperties.png)  
    * Text Colors:

    ```
    // i.e. : "#f1f1f1"
    feedManager.setMaiaContainerTitleColor("** HEX code for the color **");
    feedManager.setMaiaAppNameColor("** HEX code for the color **");
    feedManager.setSponsorColor("** HEX code for the color **");
    feedManager.setDescriptionColor("** HEX code for the color **");
    feedManager.setMaiaTaglineColor("** HEX code for the color **");
    feedManager.setMaiaDownloadInfoColor("** HEX code for the color **");
    feedManager.setMaiaCTAColor("** HEX code for the color **");
    ```  

    * Text Size:

    ```
    // i.e : 14
    feedManager.setMaiaContainerTitleSize(** Size **);
    feedManager.setMaiaAppNameSize(** Size **);
    feedManager.setSponsorSize(** Size **);
    feedManager.setDescriptionSize(** Size **);
    feedManager.setMaiaTaglineSize(** Size **);
    feedManager.setMaiaDownloadInfoSize(** Size **);
    feedManager.setMaiaCTASize(** Size **);
    ```  

    * Text Font:

    ```
    // Define the typeface for the font
    // You can define multiple typefaces
    Typeface typeFace = Typeface.createFromAsset(getAssets(), "** Your Font **");
    Typeface typeFace2 = Typeface.createFromAsset(getAssets(), "** Your Font 2 **");

    feedManager.setMaiaContainerTitleTypeFace(typeFace);
    feedManager.setMaiaAppNameTypeFace(typeFace);
    feedManager.setSponsorTypeFace(typeFace);
    feedManager.setDescriptionTypeFace(typeFace2);
    feedManager.setMaiaTaglineTypeFace(typeFace2);
    feedManager.setMaiaDownloadInfoTypeFace(typeFace2);
    feedManager.setMaiaCTATypeFace(typeFace2);
    ```  


[Seamless SDK]:http://www.mobilike.com
