# JavaScript

#### 1. Install through "script tag"  

=== "HTML"
```HTML
<script src="https://sdk.portalapp.games/sdk.umd.js"></script>
```


#### 2. Initialize the SDK
Call the initialization functions at the start of your application:

=== "JavaScript"
```JS
await window.PortalSDK.initialize();
```

#### 2.1. Bot ID Parameter

The `initialize()` method also accepts a `botId` as the first parameter:

=== "JavaScript"
```JS
await window.PortalSDK.initialize(34398689);
```

**When do you need to specify botId?**

The `botId` parameter is **not necessary** if you're using our [hosting and upload process](/upload-game/0-upload-game/). However, if you're hosting the game on your own server (which we don't recommend), you **must** add the `botId` parameter - it's important for authentication.

To find your game's bot ID, see [How to find your game bot ID](/integration/telegram-botid/).

#### 2.2. Initialize Overlay

Initialize overlay with default options or with [startup configuration](/integration/startup-configuration/):

=== "JavaScript"
```JS
window.PortalSDK.initializeOverlay();
```


#### 3. Call game-ready event  
  Call this method when the game is ready and visible to the user.  
=== "JavaScript"  
```js
window.PortalSDK.gameReady()
```  
  
**Notes:**
*Initialization should occur as early as possible in your application lifecycle to prevent delays.*

## Local Testing

For instructions on how to test your game locally with full PortalSDK functionality (overlay, ads, IAPs), see the [Local Testing Guide](/setup/local-testing/).
