---
title: Overview of WebView2 features and APIs
description: Overview of WebView2 features and APIs.
author: MSEdgeTeam
ms.author: msedgedevrel
ms.topic: conceptual
ms.prod: microsoft-edge
ms.technology: webview
ms.date: 07/07/2022
---
# Overview of WebView2 features and APIs

<!-- todo: re-sync table sentences -->

Embedding the WebView2 control in your app gives your app access to various methods and properties that are provided through the WebView2 classes or interfaces.  WebView2 has hundreds of APIs that provide a vast set of capabilities, ranging from enhancing your app's native-platform capabilities,<!--such as [concrete/specific]--> to enabling your app to modify browser experiences.<!--reword "modify browser experiences"; ... such as [concrete/specific]-->  This article provides a high-level grouping of the WebView2 APIs to help you understand the different things you can do using WebView2.

When hosting the WebView2 control, your app has access to the following browser features and APIs:

| Feature area | Purpose |
|---|---|
| [Main classes: Environment, Controller, and Core](#main-classes-environment-controller-and-core) | The `CoreWebView2Environment`, `CoreWebView2Controller`, and `CoreWebView2` classes (or equivalent interfaces) work together so your app can host a WebView2 browser control and access its browser features.  These large classes expose a wide range of APIs that your host app can access to provide the below categories of browser-related features for your users. |
| [Web/native interop](#webnative-interop) | Embed web content into native applications.  Communicate between native code and web code using simple messages, JavaScript code, and native objects. |
| [Browser features](#browser-features) | Toggle and change these inherited features that are inherited from the browser and are available in a WebView2 control. |
| [Process management](#process-management) | Get information about running WebView2 processes, exiting processes, and failed processes, so your app can take action accordingly. |
| [Navigate to pages and manage loaded content](#navigate-to-pages-and-manage-loaded-content) | In the WebView2 control you can manage navigation to web pages and content loaded in the web pages. |
| [iFrames](#iframes) | Embed other webpages into your own webpage.  Detect when embedded webpages are created, detect when embedded webpages are navigating, and optionally bypass x-frame options. |
| [Authentication](#authentication) | Handle basic authentication in WebView2 controls. |
| [Rendering WebView2 in non-framework apps](#rendering-webview2-in-non-framework-apps) | Set up the WebView2 rendering system in non-framework apps, such as how the WebView2 control renders output into your host app, and how WebView2 handles input, focus, and accessibility. |
| [Rendering WebView2 using Composition](#rendering-webview2-using-composition) | For composition-based WebView2 rendering, use `CoreWebView2Environment` to create a `CoreWebView2CompositionController`.  The `CoreWebView2CompositionController` also implements all the APIs as `CoreWebView2Controller`, but also includes APIs for composition-based rendering. |
| [User data](#user-data) | Manage the user data folder (UDF), which is a folder on the user's machine.  The UDF contains data related to the host app and WebView2.  WebView2 apps use user data folders to store browser data, such as cookies, permissions, and cached resources. |
| [Performance and debugging](#performance-and-debugging) | Analyze and debug performance, handle performance-related events, and manage memory usage to increase responsiveness of your app. |
| [Chrome Developer Protocol (CDP)](#chrome-developer-protocol-cdp) | Instrument, inspect, debug, and profile Chromium-based browsers.  The Chrome DevTools Protocol is the foundation for the Microsoft Edge DevTools.  Use the Chrome DevTools Protocol for features that aren't implemented in the WebView2 platform. |


<!-- ====================================================================== -->
## Main classes: Environment, Controller, and Core

<!-- keep sync'd:
* [Main classes: Environment, Controller, and Core](overview-features-apis.md#main-classes-environment-controller-and-core) in _Overview of WebView2 features and APIs_.
* [Main classes for WebView2: Environment, Controller, and Core](environment-controller-core.md) - topmost content.
-->

The `CoreWebView2Environment`, `CoreWebView2Controller`, and `CoreWebView2` classes (or equivalent interfaces) work together so your app can host a WebView2 browser control and access its browser features.  These three large classes expose a wide range of APIs that your host app can access to provide many categories of browser-related features for your users.

*  The `CoreWebView2Environment` class represents a group of WebView2 controls that share the same WebView2 browser process, user data folder, and renderer processes.  From this `CoreWebView2Environment` class, you create pairs of `CoreWebView2Controller` and `CoreWebView2` instances.
*  The `CoreWebView2Controller` class is responsible for hosting-related functionality such as window focus, visibility, size, and input, where your app hosts the WebView2 control.
*  The `CoreWebView2` class is for the web-specific parts of the WebView2 control, including networking, navigation, script, and parsing and rendering HTML.

<!-- / keep sync'd -->

See also:
* [Main classes for WebView2: Environment, Controller, and Core](environment-controller-core.md)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2 Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2)
* [CoreWebView2Controller Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller)
* [CoreWebView2Environment Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2)
* [ICoreWebView2Controller](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller)
* [ICoreWebView2Environment](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment)

---


<!-- ====================================================================== -->
## Web/native interop

The Microsoft Edge WebView2 control lets you embed web content into native applications. You can communicate between native code and web code using simple messages, JavaScript code, and native objects. 

Some common use cases include:
*  Update the native host window title after navigating to a different website.
*  Send a native camera object and use its methods from a web app.
*  Run a dedicated JavaScript file on the web side of an application.

For more information, see:
* [Interop of native-side and web-side code](../how-to/communicate-btwn-web-native.md)
* [Call web-side code from native-side code](../how-to/javascript.md)
* [Call native-side code from web-side code](../how-to/hostobject.md)

The following are the main APIs for communicating between web and native code.


<!-- ------------------------------ -->
#### Host/web object sharing

WebView2 enables objects that are defined in native code to be passed to your app's web-side code.  _Host objects_ are any objects that are defined in native code that you choose to pass to your app's web-side code.

Host objects can be projected into JavaScript, so that you can call native object methods (or other APIs) from your app's web-side code.  For example, your app can call such APIs as a result of user interaction on the web side of your app. This way, you don't need to reimplement your native objects' APIs, such as methods or properties, in your web-side code.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.AddHostObjectToScript Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.addhostobjecttoscript)
* [CoreWebView2.RemoveHostObjectFromScript Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.removehostobjectfromscript)
* [CoreWebView2Settings.AreHostObjectsAllowed Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arehostobjectsallowed)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::AddHostObjectToScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#addhostobjecttoscript)
* [ICoreWebView2::RemoveHostObjectFromScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#removehostobjectfromscript)
* [ICoreWebView2Settings::AreHostObjectsAllowed property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_arehostobjectsallowed), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_arehostobjectsallowed)

---


<!-- ------------------------------ -->
#### Script execution

Allows host app to add JavaScript in the web content within the WebView2 control. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.AddScriptToExecuteOnDocumentCreatedAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.addscripttoexecuteondocumentcreatedasync)
* [CoreWebView2.ExecuteScriptAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.executescriptasync)
* [CoreWebView2.RemoveScriptToExecuteOnDocumentCreated Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.removescripttoexecuteondocumentcreated)
* [CoreWebView2Settings.IsScriptEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isscriptenabled)
* [CoreWebView2Frame2.ExecuteScriptAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.executescriptasync)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::AddScriptToExecuteOnDocumentCreated method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#addscripttoexecuteondocumentcreated)
* [ICoreWebView2::ExecuteScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#executescript)
* [ICoreWebView2::RemoveScriptToExecuteOnDocumentCreated method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#removescripttoexecuteondocumentcreated)
* [ICoreWebView2Settings::IsScriptEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isscriptenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isscriptenabled)
* [ICoreWebView2Frame2::ExecuteScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#executescript)

---


<!-- ------------------------------ -->
#### Web messaging

Your app can send messages to the web content that's within the WebView2 control, and receive messages from that web content.  Messages are sent as strings or JSON objects.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.PostWebMessageAsJson Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.postwebmessageasjson)
* [CoreWebView2.PostWebMessageAsString Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.postwebmessageasstring)
* [CoreWebView2.WebMessageReceived Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webmessagereceived)
   * [CoreWebView2WebMessageReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webmessagereceivedeventargs)
* [CoreWebView2Settings.IsWebMessageEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.iswebmessageenabled)
* [CoreWebView2Frame.PostWebMessageAsJson Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.postwebmessageasjson)
* [CoreWebView2Frame.PostWebMessageAsString Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.postwebmessageasstring)
* [CoreWebView2Frame.WebMessageReceived Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.webmessagereceived)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::PostWebMessageAsJson method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#postwebmessageasjson)
* [ICoreWebView2::PostWebMessageAsString method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#postwebmessageasstring)
* [ICoreWebView2::WebMessageReceived event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_webmessagereceived), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_webmessagereceived)
   * [ICoreWebView2WebMessageReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webmessagereceivedeventargs)
* [ICoreWebView2Settings::IsWebMessageEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_iswebmessageenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_iswebmessageenabled)
* [ICoreWebView2Frame2::PostWebMessageAsJson method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#postwebmessageasjson)
* [ICoreWebView2Frame2::PostWebMessageAsString method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#postwebmessageasstring)
* [ICoreWebView2Frame2::WebMessageReceived event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#add_webmessagereceived), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#remove_webmessagereceived)

---


<!-- ------------------------------ -->
#### Script dialogs

When hosting WebView2, your app can manage different JavaScript dialogs, to suppress them or to replace them with custom dialogs.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.ScriptDialogOpening Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.scriptdialogopening)
   * [CoreWebView2ScriptDialogOpeningEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2scriptdialogopeningeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::ScriptDialogOpening event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_scriptdialogopening), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_scriptdialogopening)
   * [ICoreWebView2ScriptDialogOpeningEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2scriptdialogopeningeventargs)

---


<!-- ====================================================================== -->
## Browser features

The WebView2 control gives your host app access to many browser features. You can modify or turn off these features.


<!-- ------------------------------ -->
#### Printing

You can configure custom print settings and print to PDF.  

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.PrintToPdfAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.printtopdfasync)
* [CoreWebView2Environment.CreatePrintSettings Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createprintsettings)
* [CoreWebView2PrintSettings Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2printsettings)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2_7::PrintToPdf method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_7#printtopdf)
* [ICoreWebView2Environment6::CreatePrintSettings method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment6#createprintsettings)
* [ICoreWebView2PrintSettings interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2printsettings)

---


<!-- ------------------------------ -->
#### Cookies

You can use cookies in WebView2 to manage user sessions, store user personalization preferences, and track user behavior.

See also:
* [View, edit, and delete cookies](/microsoft-edge/devtools-guide-chromium/storage/cookies)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Cookie Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookie)
* [CoreWebView2CookieList Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookielist)
* [CoreWebView2.CookieManager Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cookiemanager#microsoft-web-webview2-core-corewebview2-cookiemanager)
   * [CoreWebView2CookieManager Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookiemanager)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Cookie interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookie)
* [ICoreWebView2CookieList interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookielist)
* [ICoreWebView2_2.CookieManager property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#get_cookiemanager)
   * [ICoreWebView2CookieManager interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookiemanager)

---


<!-- ------------------------------ -->
#### Image capture

By hosting WebView2, your app can capture screenshots and indicate which format to use to save the image.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.CapturePreviewAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.capturepreviewasync)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2.CapturePreview method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#capturepreview)

---


<!-- ------------------------------ -->
#### Downloads

Your app can manage the download experience in WebView2.  Your app can:
*  Allow or block downloads based on different metadata.
*  Change the download location.
*  Configure a custom download UI.
*  Customize the default UI.

##### [.NET/C#](#tab/dotnetcsharp)

General:
* [CoreWebView2.IsDefaultDownloadDialogOpenChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdefaultdownloaddialogopenchanged)
* [CoreWebView2.IsDefaultDownloadDialogOpen Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdefaultdownloaddialogopen)
* [CoreWebView2.OpenDefaultDownloadDialog Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opendefaultdownloaddialog)
* [CoreWebView2.CloseDefaultDownloadDialog Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.closedefaultdownloaddialog)

Modify Default Experience:
* [CoreWebView2.DefaultDownloadDialogCornerAlignment Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.defaultdownloaddialogcorneralignment)
* [CoreWebView2.DefaultDownloadDialogMargin Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.defaultdownloaddialogmargin)
* [CoreWebView2Profile.DefaultDownloadFolderPath Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2profile.defaultdownloadfolderpath)

Custom Download Experience:
* [CoreWebView2.DownloadStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.downloadstarting)
   * [CoreWebView2DownloadStartingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadstartingeventargs)
* [CoreWebView2DownloadOperation Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadoperation)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

General:
* [ICoreWebView2_9::IsDefaultDownloadDialogOpen property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_isdefaultdownloaddialogopen)<!--no put-->
* [ICoreWebView2_9::OpenDefaultDownloadDialog method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#opendefaultdownloaddialog)
* [ICoreWebView2_9::IsDefaultDownloadDialogOpenChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#add_isdefaultdownloaddialogopenchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#remove_isdefaultdownloaddialogopenchanged)
* [ICoreWebView2_9::CloseDefaultDownloadDialog method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#closedefaultdownloaddialog)

Modify Default Experience:
* [ICoreWebView2_9::DefaultDownloadDialogCornerAlignment property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_defaultdownloaddialogcorneralignment), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#put_defaultdownloaddialogcorneralignment)
* [ICoreWebView2_9::DefaultDownloadDialogMargin property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_defaultdownloaddialogmargin), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#put_defaultdownloaddialogmargin)
* [ICoreWebView2Profile::DefaultDownloadFolderPath property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile#get_defaultdownloadfolderpath), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile#put_defaultdownloadfolderpath)

Custom Download Experience:
* [ICoreWebView2_4::DownloadStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#add_downloadstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#remove_downloadstarting)
   * [ICoreWebView2DownloadStartingEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2downloadstartingeventargs)
* [ICoreWebView2DownloadOperation interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2downloadoperation)

---


<!-- ------------------------------ -->
#### Permissions

Different web pages may ask you for permissions to access some privileged resources, such as geolocation sensor, camera, and microphone.  By using the WebView2 permissions APIs, your host app can programmatically respond to permissions requests and/or replace the default permissions UI with its own UI. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.PermissionRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.permissionrequested)
   * [CoreWebView2PermissionRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2permissionrequestedeventargs)
* [CoreWebView2Frame.PermissionRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.permissionrequested)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::PermissionRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_permissionrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_permissionrequested)
   * [ICoreWebView2PermissionRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2permissionrequestedeventargs)
* [ICoreWebView2Frame3::PermissionRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame3#add_permissionrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame3#remove_permissionrequested)

---


<!-- ------------------------------ -->
#### Context menus

The WebView2 control provides a default context menu (right-click menu) which you can customize or disable, and you can also create your own context menu. 

See also:
* [Customize context menus in WebView2](../how-to/context-menus.md)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Environment.CreateContextMenuItem Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcontextmenuitem)
* [CoreWebView2ContextMenuItem Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2contextmenuitem)
* [CoreWebView2ContextMenuTarget Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2contextmenutarget)
* [CoreWebView2.ContextMenuRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.contextmenurequested)
   * [CoreWebView2ContextMenuRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2contextmenurequestedeventargs)
* [CoreWebView2Settings.AreDefaultContextMenusEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.aredefaultcontextmenusenabled)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Environment9::CreateContextMenuItem method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment9#createcontextmenuitem)
* [ICoreWebView2ContextMenuItem interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contextmenuitem)
   * [ICoreWebView2ContextMenuItemCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contextmenuitemcollection)
* [ICoreWebView2ContextMenuTarget interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contextmenutarget)
* [ICoreWebView2_11::ContextMenuRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#add_contextmenurequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#remove_contextmenurequested)
   * [ICoreWebView2ContextMenuRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contextmenurequestedeventargs)
* [ICoreWebView2Settings::AreDefaultContextMenusEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_aredefaultcontextmenusenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_aredefaultcontextmenusenabled)

---


<!-- ------------------------------ -->
#### Status bar

<!-- why is this desirable, what kinds of changes for example, does this mean programmatically monitor? -->
A status bar is located in the bottom left of the page and displays the state of the webpage being displayed. In WebView2 you can enable/disable the status bar, get the text in the status bar, and find out when the status bar text has changed. 

<!--
See also:
* []()
-->

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.StatusBarTextChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.statusbartextchanged)
* [CoreWebView2.StatusBarText Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.statusbartext)
* [CoreWebView2Settings.IsStatusBarEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isstatusbarenabled)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2_12::StatusBarTextChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#add_statusbartextchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#remove_statusbartextchanged)
* [ICoreWebView2_12::StatusBarText property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#get_statusbartext)
* [ICoreWebView2Settings::IsStatusBarEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isstatusbarenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isstatusbarenabled)

---


<!-- ------------------------------ -->
#### User Agent
<!-- alt for "Learn how to" -->
<!-- why would you do this, what is benefit for end user, what's involved?  Your app can detect which XYZ and then ABC so that the X is Y. -->
The user agent is a string that represents the identity of the program on behalf of the user, such as the browser name. In WebView2, you can set the user agent.

<!--
See also:
* []()
-->

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.UserAgent Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.useragent)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings2::UserAgent property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings2#get_useragent), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings2#put_useragent)

---


<!-- ------------------------------ -->
#### Autofill

Your app can independently control whether the browser's autofill functionality is enabled for general information or for passwords.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.IsGeneralAutofillEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isgeneralautofillenabled)
* [CoreWebView2Settings.IsPasswordAutosaveEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.ispasswordautosaveenabled)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings4::IsGeneralAutofillEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#get_isgeneralautofillenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#put_isgeneralautofillenabled)
* [ICoreWebView2Settings4::IsPasswordAutosaveEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#get_ispasswordautosaveenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#put_ispasswordautosaveenabled)

---


<!-- ------------------------------ -->
#### Audio

Your app can mute and unmute all audio, and find out when audio is playing.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.IsDocumentPlayingAudioChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdocumentplayingaudiochanged)
* [CoreWebView2.IsMutedChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.ismutedchanged)
* [CoreWebView2.IsDocumentPlayingAudio Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdocumentplayingaudio)
* [CoreWebView2.IsMuted Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.ismuted)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2_8::IsDocumentPlayingAudioChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#add_isdocumentplayingaudiochanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#remove_isdocumentplayingaudiochanged)
* [ICoreWebView2_8::IsMutedChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#add_ismutedchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#remove_ismutedchanged)
* [ICoreWebView2_8::IsDocumentPlayingAudio property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#get_isdocumentplayingaudio)
* [ICoreWebView2_8::IsMuted property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#get_ismuted), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#put_ismuted)

---


<!-- ------------------------------ -->
#### Swipe gesture navigation

By hosting the WebView2 control, your app can enable or disable swiping gesture navigation on touch input-enabled devices. This gesture allows end users to:
*  Swipe left/right (swipe horizontally) to navigate to the previous or next page in the navigation history.
*  Pull to refresh (swipe vertically) the current page.

This feature is currently disabled by default in the browser.  To enable this feature in WebView2, set the `AdditionalBrowserArguments` property, specifying the `--pull-to-refresh` switch.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.IsSwipeNavigationEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isswipenavigationenabled)
* [CoreWebView2EnvironmentOptions.AdditionalBrowserArguments Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environmentoptions.additionalbrowserarguments)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings6::IsSwipeNavigationEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings6#get_isswipenavigationenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings6#put_isswipenavigationenabled)
* [ICoreWebView2EnvironmentOptions::AdditionalBrowserArguments property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions#get_additionalbrowserarguments), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions#put_additionalbrowserarguments)

---


<!-- ------------------------------ -->
#### Document title

Your app can detect when the title of the current top-level document has changed.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.DocumentTitle Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.documenttitle)
* [CoreWebView2.DocumentTitleChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.documenttitlechanged)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::DocumentTitle property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_documenttitle)<!--no put-->
* [ICoreWebView2::DocumentTitleChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_documenttitlechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_documenttitlechanged)

---


<!-- ------------------------------ -->
#### Fullscreen

In WebView2, you can find out when an HTML element enters or leaves full-screen view.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.ContainsFullScreenElement Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.containsfullscreenelement)
* [CoreWebView2.ContainsFullScreenElementChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.containsfullscreenelementchanged)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::ContainsFullScreenElement property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_containsfullscreenelement)<!--no put-->
* [ICoreWebView2::ContainsFullScreenElementChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_containsfullscreenelementchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_containsfullscreenelementchanged)

---

<!-- ------------------------------ -->
#### PDF toolbar

In the browser PDF viewer, there's a PDF-specific toolbar along the top.  In WebView2, you can hide some of the items in the PDF viewer toolbar.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.HiddenPdfToolbarItems Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.hiddenpdftoolbaritems)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings7::HiddenPdfToolbarItems property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings7#get_hiddenpdftoolbaritems), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings7#put_hiddenpdftoolbaritems)

---

<!-- ------------------------------ -->
#### Theming

In WebView2, you can customize the color theme as system, light, or dark.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Profile.PreferredColorScheme Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2profile.preferredcolorscheme)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Profile::PreferredColorScheme property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile#get_preferredcolorscheme), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile#put_preferredcolorscheme)

---

<!-- ------------------------------ -->
#### Language

You can set the default display language for WebView2 that applies to the browser UI (such as context menus and dialogs), along with setting the `accept-language` HTTP header which WebView2 sends to websites.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2EnvironmentOptions.Language Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environmentoptions.language)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2EnvironmentOptions::Language property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions#get_language), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions#put_language)

---

<!-- ------------------------------ -->
#### New window

WebView2 provides functionality to handle the JavaScript function `window.open()`.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.NewWindowRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.newwindowrequested)
   * [CoreWebView2NewWindowRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2newwindowrequestedeventargs)
   * [CoreWebView2WindowFeatures Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2windowfeatures)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::NewWindowRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_newwindowrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_newwindowrequested)
   * [ICoreWebView2NewWindowRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2newwindowrequestedeventargs)
   * [ICoreWebView2WindowFeatures interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2windowfeatures)

---


<!-- ------------------------------ -->
#### Close window

WebView2 provides functionality to handle the JavaScript function `window.close()`.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.close)
* [CoreWebView2.WindowCloseRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.windowcloserequested)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::Close method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#close)
* [ICoreWebView2::WindowCloseRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_windowcloserequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_windowcloserequested)

---


<!-- ====================================================================== -->
## Process management

Get information about running WebView2 processes, exiting processes, and failed processes, so that your app can take action accordingly.

##### [.NET/C#](#tab/dotnetcsharp)

Info:
* [CoreWebView2.BrowserProcessId Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.browserprocessid)
* [CoreWebView2Environment.GetProcessInfos Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.getprocessinfos)
* [CoreWebView2Environment.ProcessInfosChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.processinfoschanged)
* [CoreWebView2ProcessInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processinfo)

Exited:
* [CoreWebView2Environment.BrowserProcessExited Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.browserprocessexited)
   * [CoreWebView2BrowserProcessExitedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2browserprocessexitedeventargs)

Failed:
* [CoreWebView2.ProcessFailed Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.processfailed)
   * [CoreWebView2ProcessFailedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processfailedeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

Info:
* [ICoreWebView2::BrowserProcessId property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_browserprocessid)<!--no put-->
* [ICoreWebView2Environment8::GetProcessInfos method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#getprocessinfos)
* [ICoreWebView2Environment8::ProcessInfosChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#add_processinfoschanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#remove_processinfoschanged)
* [ICoreWebView2ProcessInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processinfo)
* [ICoreWebView2ProcessInfoCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processinfocollection)

Exited:
* [ICoreWebView2Environment5::BrowserProcessExited event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment5#add_browserprocessexited), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment5#remove_browserprocessexited)
   * [ICoreWebView2BrowserProcessExitedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2browserprocessexitedeventargs)

Failed:
* [ICoreWebView2::ProcessFailed event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_processfailed), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_processfailed)
   * [ICoreWebView2ProcessFailedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processfailedeventargs)

---


<!-- ====================================================================== -->
## Navigate to pages and manage loaded content

In the WebView2 control you can manage navigation to web pages and content loaded in the web pages. 


<!-- ------------------------------ -->
#### Manage content loaded into WebView2

These APIs load, stop loading, and reload content to WebView2.  The content that's loaded can be:
*  From a URL.
*  A string of HTML.
*  Local content via virtual host name to local folder mapping.
*  From a constructed network request.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.Navigate Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigate)
* [CoreWebView2.NavigateToString Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigatetostring)
* [CoreWebView2.NavigateWithWebResourceRequest Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigatewithwebresourcerequest)
* [CoreWebView2.Stop Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.stop)
* [CoreWebView2.Reload Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.reload)
* [CoreWebView2.SetVirtualHostNameToFolderMapping Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.setvirtualhostnametofoldermapping)
* [CoreWebView2.ClearVirtualHostNameToFolderMapping Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.clearvirtualhostnametofoldermapping)
* [CoreWebView2Settings.IsBuiltInErrorPageEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isbuiltinerrorpageenabled#microsoft-web-webview2-core-corewebview2settings-isbuiltinerrorpageenabled)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::Navigate method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#navigate)
* [ICoreWebView2::NavigateToString method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#navigatetostring)
* [ICoreWebView2_2::NavigateWithWebResourceRequest method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#navigatewithwebresourcerequest)
* [ICoreWebView2::Stop method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#stop)
* [ICoreWebView2::Reload method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#reload)
* [ICoreWebView2_3::SetVirtualHostNameToFolderMapping method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#setvirtualhostnametofoldermapping)
* [ICoreWebView2_3::ClearVirtualHostNameToFolderMapping method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#clearvirtualhostnametofoldermapping)
* [ICoreWebView2Settings::IsBuiltInErrorPageEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isbuiltinerrorpageenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isbuiltinerrorpageenabled)

---


<!-- ------------------------------ -->
#### Navigation history

The history methods allow back and forward navigation in WebView2, and the history events provide information about the changes in history and in WebView2's current source.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.Source Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.source)
* [CoreWebView2.SourceChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.sourcechanged)
   * [CoreWebView2SourceChangedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2sourcechangedeventargs)
* [CoreWebView2.HistoryChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.historychanged)
* [CoreWebView2.CanGoBack Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cangoback)
   * [CoreWebView2.GoBack Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.goback)
* [CoreWebView2.CanGoForward Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cangoforward)
   * [CoreWebView2.GoForward Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.goforward)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::Source property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_source)<!--no put-->
* [ICoreWebView2::SourceChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_sourcechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_sourcechanged)
   * [ICoreWebView2SourceChangedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2sourcechangedeventargs)
* [ICoreWebView2::HistoryChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_historychanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_historychanged)
* [ICoreWebView2::CanGoBack property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_cangoback)<!--no put-->
   * [ICoreWebView2::GoBack method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#goback)
* [ICoreWebView2::CanGoForward property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_cangoforward)<!--no put-->
   * [ICoreWebView2::GoForward method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#goforward)

---


<!-- ------------------------------ -->
#### Block unwanted navigating

The `NavigationStarting` event allows the app to cancel navigating to specified URLs in WebView2, including for frames.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.NavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigationstarting)
   * [CoreWebView2NavigationStartingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2navigationstartingeventargs)
* [CoreWebView2.FrameNavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationstarting)
* [CoreWebView2Frame.NavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.navigationstarting)
   * [CoreWebView2NavigationStartingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2navigationstartingeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::NavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_navigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_navigationstarting)
   * [ICoreWebView2NavigationStartingEventArgs2 interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationstartingeventargs2)<!--v2-->
* [ICoreWebView2::FrameNavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationstarting)
* [ICoreWebView2Frame2::NavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#add_navigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#remove_navigationstarting)
   * [ICoreWebView2NavigationStartingEventArgs2 interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationstartingeventargs2)<!--v2-->

---


<!-- ------------------------------ -->
#### Navigation events

With `NavigationStarting` and the other navigation events, the app can be informed of the state of navigation in WebView2.  A _navigation_ is the process for loading a new URL.

See also:
* [Navigation events for WebView2 apps](navigation-events.md)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.ContentLoading Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.contentloading)
   * [CoreWebView2ContentLoadingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2contentloadingeventargs)
* [CoreWebView2.DOMContentLoaded Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.domcontentloaded)
   * [CoreWebView2DOMContentLoadedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2domcontentloadedeventargs)
* [CoreWebView2.NavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigationcompleted)
   * [CoreWebView2NavigationCompletedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2navigationcompletedeventargs)
* [CoreWebView2.FrameNavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationcompleted)
* [CoreWebView2Frame.ContentLoading Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.contentloading)
* [CoreWebView2Frame.DOMContentLoaded Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.domcontentloaded)
* [CoreWebView2Frame.NavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame.navigationcompleted)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::ContentLoading event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_contentloading), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_contentloading)
   * [ICoreWebView2ContentLoadingEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contentloadingeventargs)
* [ICoreWebView2_2::DOMContentLoaded event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#add_domcontentloaded), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#remove_domcontentloaded)
   * [ICoreWebView2DOMContentLoadedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2domcontentloadedeventargs)
* [ICoreWebView2::NavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_navigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_navigationcompleted)
   * [ICoreWebView2NavigationCompletedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationcompletedeventargs)
   * [ICoreWebView2NavigationCompletedEventArgs2 interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationcompletedeventargs2)
* [ICoreWebView2::FrameNavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationcompleted)
* [ICoreWebView2Frame2::ContentLoading event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#add_contentloading), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#remove_contentloading)
* [ICoreWebView2Frame2::DOMContentLoaded event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#add_domcontentloaded), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#remove_domcontentloaded)
* [ICoreWebView2Frame2::NavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#add_navigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame2#remove_navigationcompleted)

---


<!-- ------------------------------ -->
#### Manage network requests in WebView2

The `WebResourceRequested` event allows the app to intercept and override all network requests in WebView2.  The `WebResourceResponseReceived` event allows the app to monitor the request sent to and the response received from the network.

See also:
* [Custom management of network requests](/microsoft-edge/webview2/how-to/webresourcerequested)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.WebResourceRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webresourcerequested)
   * [CoreWebView2WebResourceRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webresourcerequestedeventargs)
* [CoreWebView2.WebResourceResponseReceived Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webresourceresponsereceived)
   * [CoreWebView2WebResourceResponseReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webresourceresponsereceivedeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2::WebResourceRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_webresourcerequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_webresourcerequested)
   * [ICoreWebView2WebResourceRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webresourcerequestedeventargs)
* [ICoreWebView2_2::WebResourceResponseReceived event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#add_webresourceresponsereceived), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#remove_webresourceresponsereceived)
   * [ICoreWebView2WebResourceResponseReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webresourceresponsereceivedeventargs)

---


<!-- ------------------------------ -->
#### Client certificates

In WebView2, you can use the Client Certificate API to select the client certificate at the application level.  This API allows you to:
*  Display a UI to the user, if desired.
*  Replace the default client certificate dialog prompt.
*  Programmatically query the certificates.
*  Select a certificate from the list to respond to the server, when WebView2 is making a request to an HTTP server that needs a client certificate for HTTP authentication. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2ClientCertificate Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2clientcertificate)
* [CoreWebView2.ClientCertificateRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.clientcertificaterequested)
   * [CoreWebView2ClientCertificateRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2clientcertificaterequestedeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2ClientCertificate interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificate)
   * [ICoreWebView2ClientCertificateCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificatecollection)<!--n/a for c#-->
* [ICoreWebView2_5::ClientCertificateRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_5#add_clientcertificaterequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_5#remove_clientcertificaterequested)
   * [ICoreWebView2ClientCertificateRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificaterequestedeventargs)

---

<!-- ------------------------------ -->
#### Server certificates

In WebView2, you can use the Server Certificate API to trust the server's TLS certificate at the application level.  This way, your host app can render the page without prompting the user about the TLS error, or your host app can automatically cancel the request. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.ServerCertificateErrorDetected Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.servercertificateerrordetected)
* [CoreWebView2.ClearServerCertificateErrorActionsAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.clearservercertificateerroractionsasync)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2_14::ServerCertificateErrorDetected event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_14#add_servercertificateerrordetected), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_14#remove_servercertificateerrordetected)
* [ICoreWebView2_14::ClearServerCertificateErrorActions method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_14#clearservercertificateerroractions)

---


<!-- ====================================================================== -->
## iFrames

iFrames allow you to embed other webpages into your own webpage.  In WebView2, you can:
*  Find out when iFrames are created.
*  Find out when iFrames are navigating.
*  Allow bypassing x-frame options.

<!-- wording per overview table:
Embed other webpages into your own webpage.  Detect when embedded webpages are created, detect when embedded webpages are navigating, and optionally bypass x-frame options. -->

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Frame Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame)
* [CoreWebView2FrameInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frameinfo)
* [CoreWebView2.FrameCreated Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framecreated)
   * [CoreWebView2FrameCreatedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2framecreatedeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Frame interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame)
* [ICoreWebView2FrameInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfo)
   * [ICoreWebView2FrameInfoCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfocollection)<!--n/a for c#-->
   * [ICoreWebView2FrameInfoCollectionIterator interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfocollectioniterator)<!--n/a for c#-->
* [ICoreWebView2_4::FrameCreated event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#add_framecreated), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#remove_framecreated)
   * [ICoreWebView2FrameCreatedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2framecreatedeventargs)

---


<!-- ====================================================================== -->
## Authentication

<!-- selling point / value prop: easy configuration of WebView2 apps - support user accounts -->

Learn how to handle basic authentication in WebView2 controls.
<!-- what's the benefit for end users?  how does it essentially work? what's involved? -->

See also:
* [Basic authentication for WebView2 apps](basic-authentication.md)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2HttpRequestHeaders Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httprequestheaders)
* [CoreWebView2.BasicAuthenticationRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.basicauthenticationrequested)
   * [CoreWebView2BasicAuthenticationRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2basicauthenticationrequestedeventargs)
* [CoreWebView2BasicAuthenticationResponse Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2basicauthenticationresponse)
   * [CoreWebView2HttpResponseHeaders Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httpresponseheaders)
* [CoreWebView2HttpHeadersCollectionIterator Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httpheaderscollectioniterator)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2HttpRequestHeaders interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httprequestheaders)
* [ICoreWebView2_10::BasicAuthenticationRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_10#add_basicauthenticationrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_10#remove_basicauthenticationrequested)
   * [ICoreWebView2BasicAuthenticationRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2basicauthenticationrequestedeventargs)
* [ICoreWebView2BasicAuthenticationResponse interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2basicauthenticationresponse)
   * [ICoreWebView2HttpResponseHeaders interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httpresponseheaders)
* [ICoreWebView2HttpHeadersCollectionIterator interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httpheaderscollectioniterator)

---


<!-- ====================================================================== -->
## Rendering WebView2 in non-framework apps

<!-- keep old intro?:
These APIS are used to set up the WebView2 rendering system in non-framework apps.  For example, how WebView2 renders output into the host app, how WebView2 handles input, focus, and also (for C++ only) accessibility.
-->

If you're using a UI framework for your app, you should use the WebView2 element for that UI framework.  If you're not using a UI framework for your app (for example, if you're using pure Win32 directly), or your UI framework doesn't have a WebView2 element, then you need to create `CoreWebView2Controller` and render it into your app.  If your app UI is build using `DirectComposition` or `Windows.UI.Composition`, then you should use `CoreWebView2CompositionController`; otherwise you should use `CoreWebView2Controller`.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.CoreWebView2 Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.corewebview2)
* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.close)
* [CoreWebView2Environment.CreateCoreWebView2ControllerAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2controllerasync)<!--2 overloads-->

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::CoreWebView2 property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_corewebview2)<!--no put-->
* [ICoreWebView2Controller::Close method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#close)
* [ICoreWebView2Environment::CreateCoreWebView2Controller method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment#createcorewebview2controller)

---


<!-- ------------------------------ -->
#### Sizing, positioning, and visibility

<!-- from the removed "window mgmt" h2 section:
WebView2 gives your app access to window-specific attributes, such as positioning, focus, and keyboard accelerators. -->

`CoreWebView2Controller` takes a parent `HWND`. The `Bounds` property sizes and positions the WebView2 relative to the parent `HWND`. The visibility of WebView2 can be toggled using `IsVisible`.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.Bounds Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.bounds)
* [CoreWebView2Controller.IsVisible Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.isvisible)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::Bounds property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_bounds), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_bounds)
* [ICoreWebView2Controller::IsVisible property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_isvisible), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_isvisible)

---


<!-- ------------------------------ -->
#### Zooming

WebView2 `ZoomFactor` is used to scale just the web content of the window.  UI scaling is also updated when the user zooms the content by pressing `Ctrl` while rotating the mouse wheel.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.ZoomFactor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactor)
* [CoreWebView2Controller.ZoomFactorChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactorchanged)
* [CoreWebView2Controller.SetBoundsAndZoomFactor Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.setboundsandzoomfactor)

Browser/gesture/zoom features:<!-- moved from Rendering section - fits best in "gestures", or "zoom" list? -->
* [CoreWebView2Settings.IsPinchZoomEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.ispinchzoomenabled)
* [CoreWebView2Settings.IsZoomControlEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.iszoomcontrolenabled)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::ZoomFactor property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_zoomfactor), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_zoomfactor)
* [ICoreWebView2Controller::ZoomFactorChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_zoomfactorchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_zoomfactorchanged)
* [ICoreWebView2Controller::SetBoundsAndZoomFactor method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#setboundsandzoomfactor)

Browser/gesture/zoom features:<!-- moved from Rendering section - fits best in "gestures", or "zoom" list? -->
* [ICoreWebView2Settings5::IsPinchZoomEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings5#get_ispinchzoomenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings5#put_ispinchzoomenabled)
* [ICoreWebView2Settings::IsZoomControlEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_iszoomcontrolenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_iszoomcontrolenabled)

---


<!-- ------------------------------ -->
#### Rasterization scale

The RasterizationScale API scales all WebView2 UI including context menus, tooltip, and popups.  The app can set whether the WebView2 should detect monitor scale changes and automatically update the RasterizationScale.  `BoundsMode` is used to configure whether the Bounds property is interpreted as raw pixels, or DIPs (which need to be scaled by `RasterizationScale`).

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.BoundsMode Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.boundsmode)
* [CoreWebView2Controller.RasterizationScale Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscale)
* [CoreWebview2Controller.RasterizationScaleChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscalechanged)
* [CoreWebview2Controller.ShouldDetectMonitorScaleChanges Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.shoulddetectmonitorscalechanges)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller3::BoundsMode property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_boundsmode), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_boundsmode)
* [ICoreWebView2Controller3::RasterizationScale property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_rasterizationscale), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_rasterizationscale)
* [ICoreWebView2Controller3::RasterizationScaleChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#add_rasterizationscalechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#remove_rasterizationscalechanged)
* [ICoreWebView2Controller3::ShouldDetectMonitorScaleChanges property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_shoulddetectmonitorscalechanges), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_shoulddetectmonitorscalechanges)

---


<!-- ------------------------------ -->
#### Focus and tabbing

The WebView2 control raises events to let the app know when the control gains focus or loses focus. For tabbing (pressing the `Tab` key), there's an API to move focus into WebView2 and an event for WebView2 to request the app to take focus back.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebview2Controller.MoveFocus Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocus)
* [CoreWebview2Controller.MoveFocusRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocusrequested)
   * [CoreWebView2MoveFocusRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2movefocusrequestedeventargs)
* [CoreWebview2Controller.GotFocus Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.gotfocus)
* [CoreWebview2Controller.LostFocus Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.lostfocus)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebview2Controller::MoveFocus method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#movefocus)
* [ICoreWebview2Controller::MoveFocusRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_movefocusrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_movefocusrequested)
   * [ICoreWebView2MoveFocusRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2movefocusrequestedeventargs)
* [ICoreWebview2Controller::GotFocus event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_gotfocus), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_gotfocus)
* [ICoreWebview2Controller::LostFocus event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_lostfocus), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_lostfocus)

---


<!-- ------------------------------ -->
#### Parent window

WebView2 can be reparented to a different parent window handle (`HWND`).  WebView2 also needs to be notified when the app's position on the screen has changed.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebview2Controller.NotifyParentWindowPositionChanged Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.notifyparentwindowpositionchanged)
   * [CoreWebview2Controller.ParentWindow Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.parentwindow)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebview2Controller::NotifyParentWindowPositionChanged method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#notifyparentwindowpositionchanged)
   * [ICoreWebview2Controller::ParentWindow property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_parentwindow), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_parentwindow)

---


<!-- ------------------------------ -->
#### Keyboard accelerators

When WebView2 has focus, it directly receives input from the user. An app may want to intercept and handle certain accelerator key combinations, or disable the normal browser accelerator key behaviors.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.AreBrowserAcceleratorKeysEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arebrowseracceleratorkeysenabled)
* [CoreWebView2Controller.AcceleratorKeyPressed Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.acceleratorkeypressed)
   * [CoreWebView2AcceleratorKeyPressedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2acceleratorkeypressedeventargs)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings3::AreBrowserAcceleratorKeysEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings3#get_arebrowseracceleratorkeysenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings3#put_arebrowseracceleratorkeysenabled)
* [ICoreWebView2Controller::AcceleratorKeyPressed event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_acceleratorkeypressed), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_acceleratorkeypressed)
   * [ICoreWebView2AcceleratorKeyPressedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2acceleratorkeypressedeventargs)

---


<!-- ------------------------------ -->
#### Default background color

WebView2 can specify a default background color.  The color can be any opaque color, or transparent.  This color will be used if the HTML page doesn't set its own background color.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.DefaultBackgroundColor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.defaultbackgroundcolor)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller2::DefaultBackgroundColor property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller2#get_defaultbackgroundcolor), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller2#put_defaultbackgroundcolor)

---


<!-- ====================================================================== -->
## Rendering WebView2 using Composition

For composition-based WebView2 rendering, use `CoreWebView2Environment` to create a `CoreWebView2CompositionController`.  The `CoreWebView2CompositionController` also implements all the APIs as `CoreWebView2Controller`, but also includes APIs for composition-based rendering.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller)
* [CoreWebView2Environment.CreateCoreWebView2CompositionControllerAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2compositioncontrollerasync)<!--2 overloads-->

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller)
* [ICoreWebView2Environment3::CreateCoreWebview2CompositionController method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment3#createcorewebview2compositioncontroller)

---


<!-- ------------------------------ -->
#### Connecting to the visual tree

WebView2 can connect its composition tree to an [IDCompositionVisual](https://docs.microsoft.com/windows/win32/api/dcomp/nn-dcomp-idcompositionvisual), [IDCompositionTarget](https://docs.microsoft.com/windows/win32/api/dcomp/nn-dcomp-idcompositiontarget), or `Windows::UI::Composition::ContainerVisual`.<!--link these?  both plats?  not linked in wv2 api ref.  these are c++ not .net links -->

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController.RootVisualTarget Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.rootvisualtarget)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController::RootVisualTarget property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_rootvisualtarget), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#put_rootvisualtarget)

---

<!-- ------------------------------ -->
#### Forwarding input

Spatial input (mouse, touch, pen) is received by the application and must be sent to WebView2.  WebView2 notifies the app when the cursor should be updated based on the mouse position.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController.Cursor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursor)
* [CoreWebView2CompositionController.CursorChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursorchanged)
* [CoreWebView2CompositionController.SystemCursorId Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.systemcursorid)
* [CoreWebView2CompositionController.SendMouseInput Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendmouseinput)
* [CoreWebView2CompositionController.SendPointerInput Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendpointerinput)
* [CoreWebView2Environment.CreateCoreWebView2PointerInfo Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2pointerinfo)
   * [CoreWebView2PointerInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2pointerinfo)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController::Cursor property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_cursor)<!--no put-->
* [ICoreWebView2CompositionController::CursorChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#add_cursorchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#remove_cursorchanged)
* [ICoreWebView2CompositionController::SystemCursorId property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_systemcursorid)<!--no put-->
* [ICoreWebView2CompositionController::SendMouseInput method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#sendmouseinput)
* [ICoreWebView2CompositionController::SendPointerInput method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#sendpointerinput)
* [ICoreWebView2Environment3::CreateCoreWebView2PointerInfo method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment3#createcorewebview2pointerinfo)
   * [ICoreWebView2PointerInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2pointerinfo)

---

<!-- ------------------------------ -->
#### Accessibility

By default, WebView2 will show up in the accessibility tree as a child of the parent HWND, for Win32/C++ apps.  WebView2 provides API to better position the WebView2 content relative to other elements in the application.

##### [.NET/C#](#tab/dotnetcsharp)

Not applicable.

##### [WinRT/C#](#tab/winrtcsharp)

Not applicable.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController2::AutomationProvider property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller2#get_automationprovider)<!--no put-->
* [ICoreWebView2Environment4::GetAutomationProviderForWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment4#getautomationproviderforwindow)<!--not in c#-->

---

<!-- ====================================================================== -->
## User data

Manage the user data folder (UDF), which is a folder on the user's machine.  The UDF contains data related to the host app and WebView2.  WebView2 apps use user data folders to store browser data, such as cookies, permissions, and cached resources.

See also:
* [Manage user data folders](user-data-folder.md)

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Environment.UserDataFolder Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.userdatafolder)
* [CoreWebView2EnvironmentOptions.ExclusiveUserDataFolderAccess Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environmentoptions.exclusiveuserdatafolderaccess)
* [CoreWebView2Profile.ClearBrowsingDataAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2profile.clearbrowsingdataasync)
* [CoreWebView2Environment.CreateCoreWebView2CompositionControllerAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2compositioncontrollerasync)
* [CoreWebView2Environment.CreateCoreWebView2ControllerOptions Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2controlleroptions)
* [CoreWebView2Environment.CreateCoreWebView2ControllerWithOptions Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2controllerasync)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Environment7::UserDataFolder property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment7#get_userdatafolder)<!--no put-->
* [ICoreWebView2EnvironmentOptions2::ExclusiveUserDataFolderAccess property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions2#get_exclusiveuserdatafolderaccess), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions2#put_exclusiveuserdatafolderaccess)
* [ICoreWebView2Profile2::ClearBrowsingData method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile2#clearbrowsingdata)
* [ICoreWebView2Profile2::ClearBrowsingDataAll method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile2#clearbrowsingdataall)
* [ICoreWebView2Profile2::ClearBrowsingDataInTimeRange method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2profile2#clearbrowsingdataintimerange)
* [ICoreWebView2Environment10::CreateCoreWebView2CompositionControllerWithOptions method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment10#createcorewebview2compositioncontrollerwithoptions)<!-- c#: CreateCoreWebView2CompositionControllerAsync -->
* [ICoreWebView2Environment10::CreateCoreWebView2ControllerOptions method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment10#createcorewebview2controlleroptions)
* [ICoreWebView2Environment10::CreateCoreWebView2ControllerWithOptions method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment10#createcorewebview2controllerwithoptions)

---

<!-- ====================================================================== -->
## Performance and debugging

Analyze and debug performance, handle performance-related events, and manage memory usage to increase responsiveness of your app.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2.MemoryUsageTargetLevel Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.memoryusagetargetlevel)
* [CoreWebView2.TrySuspendAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.trysuspendasync)
   * [CoreWebView2.IsSuspended Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.issuspended)
   * [CoreWebView2.Resume Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.resume)
* [CoreWebView2.OpenTaskManagerWindow Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opentaskmanagerwindow) 

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Experimental5::MemoryUsageTargetLevel property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2experimental5#get_memoryusagetargetlevel), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2experimental5#put_memoryusagetargetlevel)
* [ICoreWebView2_3::TrySuspend method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#trysuspend)
   * [ICoreWebView2_3::IsSuspended property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#get_issuspended)<!--no put-->
   * [ICoreWebView2_3::Resume method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#resume)
* [ICoreWebView2_6::OpenTaskManagerWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_6#opentaskmanagerwindow)

---


<!-- ====================================================================== -->
## Chrome Developer Protocol (CDP)

The Chrome DevTools Protocol provides APIs to instrument, inspect, debug, and profile Chromium-based browsers.  The Chrome DevTools Protocol is the foundation for the Microsoft Edge DevTools.  Use the Chrome DevTools Protocol for features that aren't implemented in the WebView2 platform.

See also:
* [Use the Chrome DevTools Protocol in WebView2 apps](../how-to/chromium-devtools-protocol.md)
* [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol)


##### [.NET/C#](#tab/dotnetcsharp)

Open:
* [CoreWebView2Settings.AreDevToolsEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.aredevtoolsenabled)
* [CoreWebView2.OpenDevToolsWindow Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opendevtoolswindow)

Call:
* [CoreWebView2.CallDevToolsProtocolMethodAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.calldevtoolsprotocolmethodasync)
* [CoreWebView2.CallDevToolsProtocolMethodForSessionAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.calldevtoolsprotocolmethodforsessionasync)

Receiver:
* [CoreWebView2.GetDevToolsProtocolEventReceiver Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.getdevtoolsprotocoleventreceiver)
   * [CoreWebView2DevToolsProtocolEventReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2devtoolsprotocoleventreceivedeventargs)
   * [CoreWebView2DevToolsProtocolEventReceiver Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2devtoolsprotocoleventreceiver)

##### [WinRT/C#](#tab/winrtcsharp)

* [WinRT API Reference](https://docs.microsoft.com/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/) - same naming as .NET/C#.

##### [Win32/C++](#tab/win32cpp)

Open:
* [ICoreWebView2Settings::AreDevToolsEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_aredevtoolsenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_aredevtoolsenabled)
* [ICoreWebView2::OpenDevToolsWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#opendevtoolswindow)

Call:
* [ICoreWebView2::CallDevToolsProtocolMethod method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#calldevtoolsprotocolmethod)
* [ICoreWebView2_11::CallDevToolsProtocolMethodForSession method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#calldevtoolspotocolmethodforsession)

Receiver:
* [ICoreWebView2::GetDevToolsProtocolEventReceiver method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#getdevtoolsprotocoleventreceiver)
   * [ICoreWebView2DevToolsProtocolEventReceiver interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2devtoolsprotocoleventreceiver)
   * [ICoreWebView2DevToolsProtocolEventReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2devtoolsprotocoleventreceivedeventargs)

---

<!-- ====================================================================== -->
## See also

* [Introduction to Microsoft Edge WebView2](../index.md)
* [WebView2 API Reference](../webview2-api-reference.md) - API Reference links for additional platforms and languages, such as WinRT/C++ (COM).