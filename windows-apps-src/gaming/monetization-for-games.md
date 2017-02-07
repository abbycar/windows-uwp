---
author: joannaleecy
title: Monetization for games
description: Implement banner ads, interstitial video ads, and in-app purchases for Universal Windows Platform (UWP) games on Windows 10.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, games, monetization
---
#  Monetization for games

As a game developer, you need to know your monetization options so you can sustain your business and keep doing what you're passionate about: creating great games. This article provides an overview of the monetization methods for a Universal Windows Platform (UWP) game and how to implement them.

In the past, you would simply put a price on your game and then wait for people to purchase it at a store. But today you have options. You can choose to distribute a game to "brick-and-mortar" stores, sell the game online (either physical or soft copies), or let everyone play the game for free but incorporate some sort of ads or in-game items that can be purchased. Games are also no longer just standalone products. They often come with extra content that can be purchased in addition to the main game. 

You can promote and monetize a UWP game in one or more of these ways:
* Put your game in the Windows Store, which is a secured, online store offering [worldwide distribution](#worldwide-distribution-channel). Gamers around the world can buy your game online at the [price you set](#set-a-price-for-your-game).
* Use APIs in the Windows SDK to create [in-game purchases](#in-game-purchases). Gamers can buy items from within your game, or buy additional content such as extra equipment, skins, maps, or game levels.
* Use APIs in the [Microsoft Store Services SDK](https://visualstudiogallery.msdn.microsoft.com/229b7858-2c6a-4073-886e-cbb79e851211) to display ads from ad networks. You can [display ads in your game](#display-ads-in-your-game) and offer the option for gamers to watch video ads in exchange for in-game rewards.
* [Maximize your game's potential through ad campaigns](#maximize-your-games-potential-through-ad-campaigns). Promote your game using paid, community (free), or house (free) ad campaigns to grow its user base.
 
## Worldwide distribution channel

The Windows Store can make your game available for download in more than 200 countries and regions worldwide, with support for billing via various forms of payment including Visa, MasterCard, and PayPal. For a full list of countries and regions, see [Markets and custom prices](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices).

## Set a price for your game 

UWP games published to the Store can be either be _paid_ or _free_. A paid game allows you to charge gamers up front for your game at a price you set, whereas a free game allows users to download and play the game without paying for it.

Here are some important concepts regarding the pricing of your game in the Store.

### Base price 

The base price of the game is what determines whether your game is categorized as _paid_ or _free_. You can use the [Dev Center dashboard](https://developer.microsoft.com/windows) to configure the base price based on country and region. 
The process of determining the price may include your [tax responsibilities when selling to different countries](https://msdn.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps) 
and [cost considerations for specific markets](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#price-considerations-for-specific-markets). You can also [set custom prices for specific markets](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). For more info, see [Define pricing and market selection](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection).

### Sale price

One way to promote your game is to reduce its price for a limited time. It's also possible to set the sale price to __Free__ to allow your game to be downloaded without payment.
You can schedule sale campaigns in advance by setting both the starting date and ending date of the sale. For more info, see [Put apps and add-ons on sale](https://msdn.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale).

## In-game purchases

In-game purchases are products bought within a game. They're also generically known as _in-app purchases_. In the Windows Store, these products are called _add-ons_. [Add-ons are published](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions) through the Windows Dev Center dashboard. You'll also need to enable the add-ons in your game's code.

### Types of add-ons

You can create two types of add-ons in the store: _durables_ or _consumables_. Durables are items that persist over for a specified amount of time and can be purchased only once until they expire. Consumables are items that can be purchased and used again and again.

When creating consumables, decide how you want to keep track of them &mdash; that is whether they're _developer managed_ or _Store managed_ (This feature is available starting in Windows 10, version 1607). With a developer-managed consumable, you are responsible for keeping track of the item's balance for the gamer; with a Store-managed consumable, the Windows Store keeps track of the item's balance for you. For more info, see [Overview of consumable add-ons](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases#overview-of-consumable-add-ons).

### Create in-game purchases

The latest in-app purchases and license info APIs are part of the [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) namespace in the Windows SDK (starting in Windows 10, version 1607). If you're developing a new game that targets 1607 or later release, we recommend that you use the __Windows.Services.Store__ namespace because it supports the latest add-on types and has better performance.
It's also designed to be compatible with future types of products and features supported by the Windows Dev Center and the Store. When developing for previous versions of Windows 10, use the [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) namespace instead.

For more info, go to [In-app purchases and trials](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials).

#### Simplified purchase example

This section uses a simplified purchase example to illustrate the use of different method calls to implement the purchase flow.

|In-game actions / activity                                                | Game background tasks                  |
|--------------------------------------------------------------------------|----------------------------------------|
|Gamer enters a shop. Shop menu pops up to display the available add-ons and purchase price |  Game [retrieves the product info](https://msdn.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons) of the add-ons, [determines whether the add-ons have the proper license](https://msdn.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons), and displays the add-ons that are available for purchase by the gamer on the shop menu.                           |
|Gamer clicks __Buy__ to purchase an item             |The __Buy__ action sends a request to purchase the item and starts the payment process to acquire it. The implementation varies depending on the item type. If it is [a durable or a one-time purchase item](https://msdn.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons), the customer can own only a single item until it expires. If the item is a [consumable](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases), the customer can own one or more of it. |

### Test in-game purchases during game development

Because an add-on must be created in association with a game, your game must be published and available in the Store. The steps in this section show how to create add-ons while your game is still in development.
(If your finished game is already live in the Store, you can skip the first three steps and go directly to [Create an add-on in the Store](#create-an-add-on-in-the-store).)

To create add-ons while your game is still in development: 
1. [Create a package](#create-a-package)
2. [Publish the game as hidden](#publish-the-game-as-hidden)
3. [Associate your game solution in Visual Studio with the Store](#associate-your-game-solution-with-the-store)
4. [Create an add-on in the Store](#create-an-add-on-in-the-store)

#### Create a package

For any game to be published, it must meet the minimum Windows App Certification requirements. You can use the [Windows App Certification Kit](https://msdn.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit), which is part of the Windows 10 SDK, 
to run tests on the game to help ensure that it's ready for publishing to the Store. If you have not already downloaded the Windows 10 SDK that includes the Windows App Certification Kit, go to [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

To create a package that can be uploaded to the Store:

1. Open your game solution in Visual Studio.
2. Within Visual Studio, go to __Project__ > __Store__ > __Create App Packages ...__
3. For the __Do you want to build packages to upload to the Windows Store?__ option, select __Yes__.
4. Sign in to your Dev Center developer account. Or [register](https://developer.microsoft.com/store/register) for a developer account if you don't have one.
5. Select an app to create the upload package for. If you have not yet created an app submission, provide a new app name to create a new submission. For more info, see [Create your app by reserving a name](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name).
6. After the package has been created successfully, click __Launch Windows App Certification Kit__ to start the testing process.
7. Fix any errors to create a game package.

#### Publish the game as hidden

1. Go to [Dev Center](https://developer.microsoft.com/store) and sign in.
2. From the __Dashboard overview__ or __All apps__ page, click the app you want to work with. If you have not yet created an app submission, click on __Create a new app__ and reserve a name.
3. On the __App Overview__ page, click __Start your submission__.
4. Configure this new submission. On the submission page: 
    * Click __Pricing and availability__. In the __Visibility__ section, choose '__Hide this app and prevent acquisition...__' to ensure only your development team has access to the game. For more details, go to [Distribution and visibility](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#distribution-and-visibility).
    * Click __Properties__. In the __Category and subcategory__ section, choose __Games__ and then a suitable subcategory for your game.
    * Click __Age ratings__. Fill out the questionnaire accurately.
    * Click __Packages__. Upload the game package created in the earlier step.
5. Follow any other submission prompts in the dashboard to allow you to successfully publish this game which remains hidden to the public.
6. Click __Submit to the Store__.

For more info, go to [App submissions](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).

After your game is submitted to the Store, it enters the [app certification process](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process). This process can take up to 16 hours before the game is listed.

#### Associate your game solution with the Store

With your game solution opened in Visual Studio:

1. Go to __Project__ > __Store__ > __Associate App with the Store ...__
2. Sign in to your Dev Center developer account and select the app name to associate this solution with.
3. Double-click on the __Package.appxmanifest.xml file__ and go to the __Packaging__ tab to check that the game is associated correctly.

If you have associated the solution to a published game that is live and listed in the Store, your solution will have an active license and you are one step closer in creating add-ons for your game. For more info, see [Packaging apps](https://msdn.microsoft.com/windows/uwp/packaging/index).

#### Create an add-on in the Store

As you create add-ons, make sure you're associating them with the right game submission. For details about how to configure all the various info associated with an add-on, see [Add-on submissions](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

1. Go to the [Dev Center](https://developer.microsoft.com/store) and sign in.
2. From the __Dashboard overview__ or __All apps__ page, click the app you want to create the add-on for.
3. On the __App Overview__ page, in the __Add-ons__ section, select __Create a new add-on__.
4. Select the product type for the add-on: __developer-managed consumable__, __store-managed consumable__, or __durable__.
5. Enter a unique product ID which will be used as a string variable when integrating this add-on into your game code. This ID will not be seen by consumers. For more info, see [Set your app product type and product ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-add-on-product-id).

Other configurations for add-ons include:
* [Properties](https://msdn.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [Pricing and availability](https://msdn.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [Store listing](https://msdn.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

If your game has many add-ons, you can create them programmatically by using the __Windows Store submission API__. For more info, see [Create and manage submissions using Windows Store services](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services).

## Display ads in your game

The libraries and tools in the Microsoft Store Services SDK help you set up a service in your game to receive ads from an ad network. Your gamers will be shown live ads and you'll earn money from the advertisers when your gamers view or interact with the displayed ads. 
For more info, see [Workflows for creating apps with ads](https://msdn.microsoft.com/windows/uwp/monetize/workflows-for-creating-apps-with-ads).

### Ad formats

Two types of ads can be displayed by using the Microsoft Store Services SDK:

* Banner ads &mdash; Ads that take up a part of your gaming screen and are usually placed within a game.
* Interstitial video ads &mdash; Full-screen ads, which can be very effective when used between levels. If implemented properly, they can be less obtrusive than banner ads.

### Which ads are displayed?

Ads are currently served through our partner networks when you use the Microsoft Store Services SDK. For more info about current offerings, see [Monetize your apps with ads](https://developer.microsoft.com/store/monetize/ads-in-apps).
If you use AdControl to display ads, you can opt in to show [affiliate ads](https://msdn.microsoft.com/windows/uwp/publish/about-affiliate-ads) by expanding the product ads that are shown in your game.

### Which markets allow ads to be displayed?

Banner ads and interstitial video ads can be shown to users from selected countries. For the full list of countries and regions that support ads, see [Supported markets for Microsoft Advertising](https://msdn.microsoft.com/windows/uwp/monetize/supported-markets-for-microsoft-advertising).

### APIs for displaying ads

The [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)  and [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) classes in the Microsoft Store Services SDK, part of the [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx) namespace are used to help display ads in games.

To get started, download and install the [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) with Visual Studio 2015. For more info, see [Features available in the SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk#features-available-in-the-sdk).

#### Implementation guides

These walkthroughs show how to implement ads by using __AdControl__ and __InterstitialAd__:

* [Create banner ads by using the AdControl class in XAML and .NET](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [Create banner ads by using the AdControl class in HTML5 and JavaScript](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [Create interstitial video ads by using the InterstitialAd class](https://msdn.microsoft.com/windows/uwp/monetize/interstitial-ads)

During development, you can make use of these test values to see how the ads are rendered. These same values are also used in the walkthroughs above.

|AdType             | AdUnitId  | AppId                              |
|-------------------|-----------|------------------------------------|
|Banner ads         |10865270   |3f83fe91-d6be-434d-a0ae-7351c5a997f1|
|Interstitial ads	|11389925   |d25517cb-12d4-4699-8bdc-52040c712cab|

Here are some best practices to help you in the design and implementation process.

* [Best practices for banner ads by using the AdControl class](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [Best practices for interstitial ads by using the InterstitialAd class](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines#interstitialbestpractices10)

For solutions to common development issues, like ads not appearing, black box blinking and disappearing, or ads not refreshing, see [Troubleshooting guides](https://msdn.microsoft.com/windows/uwp/monetize/troubleshooting-guides).

### Prepare for release by replacing ad unit test values

When you're ready to move to live testing or to receive ads in published games, you must update the test ad unit values to the actual values provided for your game.
To create ad units for your game, see [Set up ad units in your app](https://msdn.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app).

### Other ad networks

These are other ad networks that support serving ads to UWP apps and games.

#### Vungle

The Vungle SDK for Windows offer video ads in apps and games. To download the SDK, go to [Vungle SDK](https://v.vungle.com/sdk).

#### Smaato

Smaato enables banner ads to be incorporated into UWP apps and games. Download the [SDK](https://www.smaato.com/resources/sdks/) and for more info, see the [documentation](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### AdDuplex

You can use AdDuplex to implement banner or interstitial ads in your game.

To learn more about integrating AdDuplex directly into a Windows 10 XAML project, go to the AdDuplex website:
* Banner ads: [Windows 10 SDK for XAML](https://adduplex.zendesk.com/hc/en-us/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) 
* Interstitial ads: [Windows 10 XAML AdDuplex Interstitial Ad Installation and Usage](https://adduplex.zendesk.com/hc/en-us/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

For info about integrating the AdDuplex SDK into Windows 10 UWP games created using Unity, see [Windows 10 SDK for Unity apps installation and usage](https://adduplex.zendesk.com/hc/en-us/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage).

## Maximize your game's potential through ad campaigns

Take the next step in promoting your game using ads. When you [create an ad campaign](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app) for your game, other apps and games will display ads promoting your game. 

Choose from several types of campaigns that can help increase your gamer base.

|Campaign type             | Ads for your game appear in...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Paid                      |Apps that match your game’s device or category.                                                                                                                                                   |
|Free community	           |Apps published by other developers who have also opted in to community ad campaigns. For more info, see [About community ads](https://msdn.microsoft.com/windows/uwp/publish/about-community-ads).|
|Free house	               |Only apps that you've published. For more info, see [About house ads](https://msdn.microsoft.com/windows/uwp/publish/about-house-ads).                                                            |

## Related links

* [Getting paid](https://msdn.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [Account types, locations, and fees](https://msdn.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [Analytics](https://msdn.microsoft.com/windows/uwp/publish/analytics)
* [Globalization and localization](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Implement a trial version of your app](https://msdn.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [Run app experiments with A/B testing](https://msdn.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)