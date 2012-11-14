# Setup #
1. If you haven't already, go to http://giraffegraph.com and register for an account. You will receive an API Key.
2. [Download the source code](https://dl.dropbox.com/s/cyxyqj26jkf6d47/GiraffeGraph-iOS.zip?dl=1) and extract the zip file.
3. Copy the GiraffeGraph-iOS folder into the source of your project in XCode. Check "Copy items into destination group's folder (if needed)".
4. In every file that uses analytics, import GGEventLog.h at the top:

        #import "GGEventLog.h"

5. In the application:didFinishLaunchingWithOptions: method of your YourAppNameAppDelegate.m file, initialize the SDK:

        [GGEventLog initializeApiKey:@"YOUR_API_KEY_HERE"];

6. To track an event anywhere in the app, call:

        [GGEventLog logEvent:@"EVENT_IDENTIFIER_HERE"];

7. Events are saved locally. Uploads are batched to occur every 30 events and every 30 seconds. After calling logEvent in your app, you will immediately see data appear on Giraffe Graph.

# Tracking Events #

It's important to think about what types of events you care about as a developer. You should aim to track between 5 and 50 types of events within your app. Common event types are different screens within the app, actions the user initiates (such as pressing a button), and events you want the user to complete (such as filling out a form, completing a level, or making a payment). Shoot me an email if you want assistance determining what would be best for you to track.

# Tracking Sessions #

A session is a period of time that a user has the app in the foreground. Sessions within 10 seconds of each other are merged into a single session. In the iOS SDK, sessions are tracked automatically.

# Settings Custom User IDs #

If your app has its own login system that you want to track users with, you can call setUserId: at any time:

    [GGEventLog setUserId:@"USER_ID_HERE"];

A user's data will be merged on the backend so that any events up to that point on the same device will be tracked under the same user.

You can also add the user ID as an argument to the initializeApiKey: call:
    
    [GGEventLog initializeApiKey:@"YOUR_API_KEY_HERE" userId:@"USER_ID_HERE"];

# Campaign Tracking #

Set up links for each of your campaigns on the campaigns tab at http://giraffegraph.com.

To track installs from each campaign source in your app, call initializeApiKey:trackCampaignSource: with an extra boolean argument to turn on campaign tracking:

    [GGEventLog initializeApiKey:@"YOUR_API_KEY_HERE" trackCampaignSource:YES];

If you are not using analytics, and only want campaign tracking, call enableCampaignTrackingApiKey: instead of initializeApiKey:trackCampaignSource: in the application:didFinishLaunchingWithOptions: method of your YourAppNameAppDelegate.m file:

    [GGEventLog enableCampaignTrackingApiKey:@"YOUR_API_KEY_HERE"];

You can retrieve the campaign information associated with a user by calling getCampaignInformation after you've called initializeApiKey:trackCampaignSource: or enableCampaignTrackingApiKey:

    [GGEventLog getCampaignInformation];

If the SDK has successfully pinged the server and saved the result, the @"tracked" key in the returned NSDictionary will be set to YES. You can then get the details of the campaign from the fields of the returned NSDictionary.

# Setting Custom Properties #

You can attach additional data to any event by passing a NSDictionary object as the second argument to GGEventLog:withCustomProperties:

    NSMutableDictionary *customProperties = [NSMutableDictionary dictionary];
    [customProperties setValue:@"VALUE_GOES_HERE" forKey:@"KEY_GOES_HERE"];
    [GGEventLog logEvent:@"Compute Hash" withCustomProperties:customProperties];

To add properties that are tracked in every event, you can set global properties for a user:

    NSMutableDictionary *globalProperties = [NSMutableDictionary dictionary];
    [globalProperties setValue:@"VALUE_GOES_HERE" forKey:@"KEY_GOES_HERE"];
    [GGEventLog setGlobalUserProperties:globalProperties];

# Advanced #

This SDK automatically grabs useful data from the phone, including app version, phone model, operating system version, and carrier information. If your app has location permissions, the SDK will also grab the location of the user (this will not consume a lot of battery, as it only polls for significant location changes).

User IDs are automatically generated based on device specific identifiers if not specified.

This code will work with both ARC and non-ARC projects, as preprocessor macros are used to determine which version of the compiler is being used.
