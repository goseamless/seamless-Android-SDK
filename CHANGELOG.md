Change Log
=========

16.02.2016 v3.0.0
-----
 1. DFP added to seamless Android SDK. This effects some ad event callbacks (see documentation).



30.12.2015 v2.8.0
-----
 1. Ad Choices Icon for Custom Native Ads
With the version 2.8.0, it's now mandatory to add ad choices icon for all native ads. In order to do that, you'll need to add a ImageView for this icon. See the documentation for more information.



11.12.2015 v2.7.0
-----
 1. New Structure for seamless Video Player


With the version 2.7.0, seamless Android SDK now has its own video player to display video ads and content videos. This means some there will be some changes in your code if you are upgrading from a previous verrsion of seamless Android SDK:  

> You need to remove your own video player and initialize SeamlessPlayerManager and play it. More information is in our documentation.
