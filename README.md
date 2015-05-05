seamless-Android-SDK(v2.2.1)
=========

Requirements
-----
* Minimum Android SDK 4.x (ICS and above)  
 

## [Installation](#installation-1)  



## How to use  

* Integrating Ads  
  * [Setup](#setup)
  * [Feed Ad Integration](#feed-ad-integration)
  * [Banner Integration](#banner-integration)
  * [Full-Page-Layer Ad Integration](#full-page-layer-ad-integration)
  * [Video Ad Integration](#video-ad-integration)  

* Customizing Ads  
  * [Feed Ad Customization](#feed-ad-customization-recommended)


## Frequently Asked Questions

* [FAQ](#faq)


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
              android:configChanges="keyboardHidden|orientation|screenSize" />
    <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidActivity"
              android:configChanges="keyboardHidden|orientation|screenSize" />
    <activity android:name="com.mobilike.seamless.mopub.common.MoPubBrowser"
              android:configChanges="keyboardHidden|orientation|screenSize" />
    <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidVideoPlayerActivity"
              android:configChanges="keyboardHidden|orientation|screenSize" />
    <activity android:name="com.facebook.ads.InterstitialAdActivity"
            android:configChanges="keyboardHidden|orientation" />
    <activity android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize" />
    <activity
            android:name="com.mobilike.seamless.player.org.nexage.sourcekit.vast.activity.VASTActivity"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen" />
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
        compile 'com.goseamless:seamless:2.2.1'
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
            android:configChanges="keyboardHidden|orientation|screenSize" />
  <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidActivity"
            android:configChanges="keyboardHidden|orientation|screenSize" />
  <activity android:name="com.mobilike.seamless.mopub.common.MoPubBrowser"
            android:configChanges="keyboardHidden|orientation|screenSize" />
  <activity android:name="com.mobilike.seamless.mopub.mobileads.MraidVideoPlayerActivity"
            android:configChanges="keyboardHidden|orientation|screenSize" />
  <activity android:name="com.facebook.ads.InterstitialAdActivity"
            android:configChanges="keyboardHidden|orientation" />
  <activity android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize" />
  <activity
            android:name="com.mobilike.seamless.player.org.nexage.sourcekit.vast.activity.VASTActivity"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen" />
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
    
    @Override
    public void onAdClicked(MoPubView view, String clickInfo) {
        // Informs you that ad is clicked
        // clickInfo returns ad's click URL. Do not use without a null check.
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


* For RecyclerView define the *RecyclerFeedListener*
```
RecyclerFeedListener feedListener = new RecyclerFeedListener() {
    @Override
    public void onAdLoad(RecyclerView.Adapter adapter) {
        // If ads loaded succesfully,
        // returns an adapter that contains feed ads
        mRecyclerView.setAdapter(adapter);

    }

    @Override
    public void onAdFailed(RecyclerView.Adapter adapter){
        // If ads fail to load, returns your own adapter
        mRecyclerView.setAdapter(adapter);
    }
    
    @Override
    public void onAdClicked(MoPubView view, String clickInfo) {
        // Informs you that ad is clicked
        // clickInfo returns ad's click URL. Do not use without a null check.
    }
};
```

* Define the *RecyclerFeedManager*
```
RecyclerFeedManager feedManager = new RecyclerFeedManager.Builder(context)
        // Should specify your current content like "yourapp-yourcontent-adtype"
        // i.e.: "myapp-sports-feed"
        .entity("xxx-- Your Entity --xxx") 
        .adapter(adapter)
        .listener(feedListener)
        .recyclerView(mRecyclerView)
        .category(AdCategories.Uncategorised) // Select proper category eg: News, Sports etc.
        .build();
```  


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
    
    @Override
    public void onBannerClicked(MoPubView view, String clickInfo) {
        // Informs you that ad is clicked
        // clickInfo returns ad's click URL. Do not use without a null check.
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
    
    @Override
    public void onInterstitialClicked(MoPubInterstitial moPubInterstitial, String clickInfo) {
        // Informs you that ad is clicked
        // clickInfo returns ad's click URL. Do not use without a null check.
    }

    @Override
    public void onInterstitialDismissed(MoPubInterstitial moPubInterstitial) {

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

>Don't forget to destroy your adView in *onDestroy()* callback
```
@Override
protected void onDestory() {
    interstitialManager.destroyAd();
    super.onDestroy();
}
```  
>For enabling in-app-redirecting to a fragment in your app, append "fragment://" pattern as a prefix for your URL.

### Video Ad Integration
Currently, Seamless Android SDK only supports preroll ads.  

* Define the *SeamlessPlayerManagerListener*  
```
SeamlessPlayerManagerListener seamlessPlayerManagerListener = new SeamlessPlayerManagerListener() {

    @Override
    public void onAdReady(VASTPlayer vastPlayer) {
        if (vastPlayer != null) {
            vastPlayer.play();
        }
    }

    @Override
    public void onAdFailed(String error) {
        // Ad Failed
    }

    @Override
    public void onAdClicked() {
        // Ad Clicked
    }

    @Override
    public void onAdCompleted() {
        // Ad Completed
    }

    @Override
    public void onAdDismissed() {
        // Ad Dismissed
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
    >
    feedManager.setMaiaContainerTitleTypeFace(typeFace);
    feedManager.setMaiaAppNameTypeFace(typeFace);
    feedManager.setSponsorTypeFace(typeFace);
    feedManager.setDescriptionTypeFace(typeFace2);
    feedManager.setMaiaTaglineTypeFace(typeFace2);
    feedManager.setMaiaDownloadInfoTypeFace(typeFace2);
    feedManager.setMaiaCTATypeFace(typeFace2);
    ```  

### FAQ

* **What are entities and categories? Are they important?**

  - Entity names are used by Seamless to distinguish different views and determine whether it should provide ad or not. For example, you could use different entity names for main view and detail view or include menu names in the entity names.
  
  - Category is used by Mopub to provide more relevant advertisement. Accurate category names will return better ads.
  
* **An entity name can contain maximum 50 characters.**
 
* **Acceptable characters in entity names are _, +, %, ^, -, /.**

* **I can’t build my app after integrating Seamless!**

  - If the build errors say that something is missing or disabled, then you might have forgotten to include permissions. Please check the setup part of the documentation.
  
  - If you’re getting NoClassDefFoundError, then please check if you have included Seamless as a library.

* **My list view is not being updated even though feed ad was successful.**

  Are you using the adapter from the success callback? This adapter is the ultimate adapter that combines both your adapter and advertisements. Please check out our demo app for a reference.

* **I see duplicate advertisements upon paging.**

  Are you passing adManager to the adapter that already contains advertisements? You don’t need to build adManager again upon paging. Please check out our demo app for a reference.

* **I don’t see any video advertisements!**

  If you triggered ad requests by playing the video, we can define ads for those entities. Please contact our operation team.

* **App fails to build with this message: Manifest merger failed: Attribute application@icon value=(@drawable/app_icon)**

  Please remove any cached seamless data in Home(cmd+sft+’H’) - .gradle(hidden folder) - caches folder.

* **App fails to build with this message: Attribute ‘theme’ has already defined.**

  This is a known google bug. You can fix this by adding latest version of google play service dependency.

* **I’m getting runtime error: java.lang.NoClassDefFoundError.**

  Consider enabling multidex. See note below.

* **After feed ad integration, my feed view flickers or gets very slow.**

  On ad load callback, please make sure to set the adapter once. The adapter returned from the callback method includes the original adapter and advertisements, so you only need to set this adapter to your feed view.

* **NOTE: Android limits the size of apps to be below 65k methods. You can overcome this limitation by enabling multi-dex.**

  One of the most common errors caused by this limitation is NoClassFoundException. If you are getting this exception even though the class does exist, then consider enabling multidex. See more details here: [https://developer.android.com/tools/building/multidex.html](https://developer.android.com/tools/building/multidex.html)

[Seamless SDK]:http://www.mobilike.com
