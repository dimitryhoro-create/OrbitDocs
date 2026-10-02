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
await window.PortalSDK.initialize(34398689);
```

#### 2.1. Bot ID Parameter

The `initialize()` method takes your game's `botId` as the first parameter:

=== "JavaScript"
```JS
await window.PortalSDK.initialize(34398689);
```

**The botId is required.**

Pass it on our [hosting](/upload-game/0-upload-game/) and on any other host - the hosting does not supply it for you. Without it we cannot verify who is calling, so every ad request your game makes stays unverified.

The `botId` is the numeric ID of the Telegram bot your game runs under. It is **not** `window.game_id`, which is the Admin Console game ID the SDK uses as the ad zone - these are different numbers, and one does not stand in for the other.

Replace `34398689` with your own bot ID. To find it, see [How to find your game bot ID](/integration/telegram-botid/).

#### 2.2. Initialize Overlay

Initialize overlay with default options or with [startup configuration](/integration/startup-configuration/):

=== "JavaScript"
```JS
window.PortalSDK.initializeOverlay({ botId: 34398689 });
```

Pass the **same** `botId` you passed to `initialize()`. Two different IDs is a conflict that stops ad delivery altogether - worse than passing none.


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
