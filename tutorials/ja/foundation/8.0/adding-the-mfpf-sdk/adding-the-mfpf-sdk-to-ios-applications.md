---
layout: tutorial
title: Adding the MobileFirst Platform Foundation SDK to iOS Applications
breadcrumb_title: iOS SDK
relevantTo: [ios]
weight: 2
---
## Overview
The MobileFirst Platform Foundation SDK provides a set of API methods enabling a developer to implement various MobileFirst features, such as: authentication and security mechanisms, notifications, resource requests, collecting analytics data and more.

> For a complete list of MobileFirst SDK abilities [visit the user documentation](http://www-01.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html).

In this tutorial you will learn how to add the MobileFirst Native SDK using CocoaPods to either a new or existing iOS application. You will also learn how to configure the MobileFirst Server to recognize the application, as well as find information about the MobileFirst configuration files that are added to the project.

**Pre-requisites:** Xcode and MobileFirst CLI installed on the developer workstation.  
Make sure you have read the [Setting up your MobileFirst development environment](../../setting-up-the-mobilefirst-development-environment) tutorial.

#### Jump to:

- [Adding the MobileFirst Native SDK](#adding-the-mobilefirst-native-sdk)
- [Generated MobileFirst Native SDK artifacts](#generated-mobilefirst-native-sdk-artifacts)
- [Tutorials to follow next](#tutorials-to-follow-next)

### Adding the MobileFirst Native SDK
Before starting, make sure the MobileFirst Server is running.  
From **Terminal** run the command:

```bash
mfpdev server start
```

Follow the below instructions to manually add the MobileFirst Native SDK to either a new or existing Xcode project.

1. Create an Xcode project or use an existing one.  

2. Open **Terminal** and navigate to the root of the Xcode project.  

3. Run the command: 
 
    ```bash
    mfpdev app register
    ```
    
    The <code>mfpdev app register</code> CLI command first connects to the MobileFirst Server to register the application, followed by generating the <code>mfpclient.plist</code> file at the root of the Xcode project, and adding to it the metadata that identifies the MobileFirst Server.
        
    > <b>Tip:</b> The application registration can also be performed from the MobileFirst Operations Console:    
        1. Open your browser of choice and load the MobileFirst Operations Console using the address <code>http://localhost:10080/mfpconsole/</code>. You can also open the console from **Terminal** using the CLI command <code>mfpdev server console</code>.  
        2. Click on the "Create new" button next to "Applications" to create a new application and follow the on-screen instructions.  
        3. After successfully registering your application you can optionally download a "skeleton" Xcode project pre-bundled with the MobileFirst Native SDK.

4. Run the command: 
 
    ```bash
    mfpdev app pull
    ```
    The <code>mfpdev app pull</code> CLI command creates the **mobilefirst** folder at the root of the Xcode project and downloads into it the <code>application-descriptor.json</code> file, containing application configuration data.
    
    These files are further explained in the [Generated MobileFirst Native SDK artifacts](#generated-mobilefirst-native-sdk-artifacts) section below.
    
    > <b>Tip:</b> Learn more about the various CLI commands in the [Using CLI to manage MobileFirst artifacts](../../client-side-development/using-cli-to-manage-mobilefirst-artifacts/) tutorial.
        
5. The MobileFirst Native SDK is provided via CocoaPods. If [CocoaPods](http://guides.cocoapods.org) is not installed in your development environment, install it as follows:    
    - Open **Terminal** and navigate to the root of the Xcode project
    - Run the command: <code>sudo gem install cocoapods</code>
    - Run the command: <code>pod setup</code>  
    **Note:** This command may take several minutes to complete.<br><br>
    
6. Run the command: <code>pod init</code>. This creates a <code>Podfile</code>.
7. Using your favorite code editor, open the <code>Podfile</code>.
8. Comment out or remove the contents of the file.
9. Add the following lines and save the changes:

    ```xml
    source 'https://github.com/CocoaPods/Specs.git'
    pod 'IBMMobileFirstPlatformFoundation'
    ```
10. Run the command: <code>pod install</code>. This command adds the MobileFirst Native SDK, generates the Pod project, and integrates it with the Xcode project.  
    **Note:** This command may take several minutes to complete.
    
    > <b>Important</b>: From here on, use the <code>[ProjectName].<b>xcworkspace</b></code> file in order to open the project in Xcode. Do <b>not</b> use the <code>[ProjectName].<b>xcodeproj</b></code> file. A CocoaPods-based project is managed as a workspace containing the application (the executable) and the library (all project dependencies that are pulled by the CocoaPods manager).
11. Open the Xcode project by double-clicking the <b>.xcworkspace</b> file.
12. In Xcode, right-click the project and select <b>Add Files To [ProjectName]</b> and select the <code>mfpclient.plist</code> file.

Whenever you want to use the MobileFirst Native SDK, make sure that you import the MobileFirst Platform Foundation framework:

```objc
#import <IBMMobileFirstPlatformFoundation/IBMMobileFirstPlatformFoundation.h> 
```

<br>
#### Note about Swift:
> Because Swift is designed to be compatible with Objective-C you can use the MobileFirst SDK from within an iOS Swift project, too. Create a Swift project and follow the same steps, as described at the beginning of the tutorial, to integrate the MobileFirst Native SDK. Use <code>import IBMMobileFirstPlatformFoundation</code> in any class that needs to use the SDK.

#### Note about iOS 9:
> If you are developing for iOS9, [consider disabling ATS](http://iosdevtips.co/post/121756573323/ios-9-xcode-7-http-connect-server-error) in the application's <code>info.plist</code> to be able to test locally without security restrictions.

## Generated MobileFirst Native SDK artifacts
Two MobileFirst-related artifacts are available in the Xcode project after it has been integrated with the MobileFirst Native SDK: the <code>mfpclient.plist</code> file and the <code>application-descriptor.json</code> file.

### mfpclient.plist 
Located at the root of the project, this file contains server connectivity properties and is user-editable:

- <code>protocol</code> – The communication protocol to MobileFirst Server. Either <code>HTTP</code> or <code>HTPS</code>.
- <code>host</code> – The hostname of the MobileFirst Server instance.
- <code>port</code> – The port of the MobileFirst Server instance.
- <code>wlServerContext</code> – The context root path of the application on the MobileFirst Server instance.
- <code>languagePreference</code> - Sets the default language for client sdk system messages

### application-descriptor.json
Located in the **&lt;xcode-project-root-directory&gt;/mobilefirst** folder, this file contains application configuration settings such as its <code>bundleId</code> and <code>version</code> and is user-editable.

The file can be edited either locally or via the MobileFirst Operations Console.  
If edited locally, the MobileFirst Server can be updated by running the CLI command: <code>mfpdev app push</code>.  
The file can also be updated by pulling from the server its latest revision by running the CLI command: <code>mfpdev app pull</code>.

```javascript
{
    "applicationKey": {
        "bundleId": "com.sampleone.bankApp",
        "version": "1.0",
        "clientPlatform":"ios"
    },
  
    ...
    ...
    ...
 }
 ```

## Tutorials to follow next
Now that the application is integrated with the MobileFirst Native SDK you can continue reading tutorials for [Native iOS development](../../ios-tutorials/) to learn more about authentication and security, server-side development, notifications, and more.