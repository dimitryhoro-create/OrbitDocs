# Local Testing

This guide explains how to test your game locally with the PortalSDK before uploading it to the platform.

### Prerequisites

Before you begin, ensure you have:

- Access to the game in Portal ([admin console](/upload-game/admin-panel/))

### How to test your game locally

#### 1. Step: Get Authentication Data

1. Open your game in Telegram mini app
2. Open dev tools - for detailed instructions, see [how to enable DevTools in Telegram](/integration/tg-devtools/)
3. In the browser DevTools console, paste the following command:

```js
window.Telegram.WebApp.initData
```

4. Copy the output string. It will look like this:

```
user=%7B%22id%22%3A122374628%2C%22first_name%22%3A...
```

![Console output example](images/local-testing/first-image.png)

!!! danger "CRITICAL: Authentication Data Must Match"
    The `botId` and `authData` **must** be from the same Telegram mini app. Do not mix authentication data from different apps. **Without matching authentication data, local testing will NOT work.**

#### 2. Step: Modify Your Index File

Locate your game's initialization code:

=== "Unity"

    For Unity projects, you need to edit the WebGL template `index.html` file. See the [Unity setup guide](/setup/unity/) for details on WebGL templates.

=== "Defold"

    For Defold projects, you need to edit the HTML5 template. See the [Defold setup guide](/setup/defold/) for details on HTML5 templates.

=== "JavaScript"

    For plain JavaScript projects, edit your `index.html` or wherever you initialize the game.

**Original Code**

Your initialization code currently looks like this:

```js
(async function () {
  await window.PortalSDK.initialize(8433755170);
  window.PortalSDK.initializeOverlay({ botId: 8433755170 });
})();
```

**Modified Code for Local Testing**

Change it to add the `authData` from Step 1:

```js
(async function () {
  await window.PortalSDK.initialize(
    8433755170,
    {
      authData: 'user=%7B%22id%22%3A122374628%2C%22first_n...'
    }
  );
  window.PortalSDK.initializeOverlay({ botId: 8433755170 });
})();
```

Replace:
- `8433755170` with your actual bot ID (see [How to find your game bot ID](/integration/telegram-botid/))
- `'user=%7B%22id%22%3A122374628%2C%22first_n...'` with your actual `authData` string from Step 1

#### 3. Step: Build and Run

=== "Unity"

    1. In Unity, go to **File > Build Settings**
    2. Click **Build and Run**
    3. Your game will open in the browser with the overlay, ads, IAPs, and all other PortalSDK features working

    ![Game running with Portal overlay](images/local-testing/second-image.png)

=== "Defold"

    1. Go to **Project > Build HTML5**
    2. Test in your browser

    That's all!

=== "JavaScript"

    Launch your game using your preferred method (local server, file opening, etc.)

You should now see your game running locally with the Portal overlay, ads, IAPs, and all other SDK features working.

#### 4. Step: Restore Original Code Before Upload

!!! danger "Critical - Do Not Skip This Step"
    Before uploading your game to GitHub, you **must** restore the original initialization code.

Change the code back to:

```js
(async function () {
  await window.PortalSDK.initialize(8433755170);
  window.PortalSDK.initializeOverlay({ botId: 8433755170 });
})();
```

Remove the `authData` parameter only - **keep the `botId`**, production needs it. If you leave `authData` in, your game **WILL NOT work correctly** in production.

### Troubleshooting

If local testing isn't working:

1. **Verify authentication data**: Make sure the `authData` string is copied completely
2. **Check bot ID**: Ensure the bot ID matches the mini app you got the auth data from
3. **Check console errors**: Open browser DevTools and look for error messages
