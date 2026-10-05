# Promo codes

Portal can launch your game with a Battle Pass promo code. The SDK extracts the code from the launch link and passes it to your game. You decide which reward each promo code grants and implement it in your game.

## How it works

| | Web | Telegram |
| :--- | :--- | :--- |
| Where the code comes from | `__portal_promo_code` URL parameter | `start_param` in Telegram `initData` |
| When the SDK reads it | When the SDK script loads | When Telegram WebApp is ready |

A valid code is 1–128 characters: `A–Z`, `a–z`, `0–9`, `_`, `-`. If there is no code or it is invalid, the SDK returns `null` (`nil` in Lua).

The method:

- works the same on Web and Telegram;
- can be called before the SDK is initialized;
- returns the same value on every call.

!!! warning
    Do not read the code from `location.search`: on Web the SDK removes `__portal_promo_code` from the URL on load. Always use the SDK method.

## Integration

=== "JavaScript"

    Add this to your game's startup code (an `async` function, next to the [SDK initialization](/setup/0-javascript/)):

    ```JS
    const promoCode = await window.PortalSDK.getPromoCode();

    if (promoCode) {
      grantPromoReward(promoCode); // your function
    }
    ```

    **Signature:** `window.PortalSDK.getPromoCode(): Promise<string | null>`

=== "Unity"

    Add this to the `Start()` method of a script on a GameObject in your **first scene** (for example, your game bootstrap script). Make `Start()` `async`:

    ```C#
    using Orbit;
    using UnityEngine;

    public class GameBootstrap : MonoBehaviour
    {
        async void Start()
        {
            string promoCode = await PortalSDK.GetPromoCode();

            if (promoCode != null)
            {
                GrantPromoReward(promoCode); // your method
            }
        }
    }
    ```

    **Signature:** `Task<string> PortalSDK.GetPromoCode()` — returns `null` if there is no code.

    !!! warning
        Do not use `PortalSDK.GetStartParam()` for promo codes: for promo links it returns the encoded `startapp` value, not the code.

=== "Defold"

    Add this to the `init(self)` function of a script (`.script`) attached to a game object in your **bootstrap collection** (the collection set in `game.project` → `bootstrap` → `main_collection`):

    ```LUA
    function init(self)
        portalsdk.get_promo_code(function(self, promo_code)
            if promo_code then
                grant_promo_reward(promo_code) -- your function
            end
        end)
    end
    ```

    **Signature:** `portalsdk.get_promo_code(callback)` — the callback receives `(self, promo_code)`, where `promo_code` is `nil` if there is no code.

## Testing

| Check | Expected result |
| :--- | :--- |
| Open `https://<game-url>/?__portal_promo_code=TEST_CODE` | The method returns `"TEST_CODE"`, the parameter is gone from the URL |
| Launch without a code | The method returns `null` / `nil` |
| Invalid code (`?__portal_promo_code=bad code!`) | The method returns `null` / `nil` |

### Testing in Telegram

`startapp` is a parameter of a Telegram Mini App link. Telegram passes its value to the game as `start_param` in `initData`, where the SDK reads the promo code. `startapp` accepts only `A–Z`, `a–z`, `0–9`, `_`, `-`, so the promo parameter is encoded with base64url.

1. Encode `?__portal_promo_code=<code>` with **base64url** — run in a browser console:

    ```JS
    btoa("?__portal_promo_code=TEST_CODE").replace(/\+/g, "-").replace(/\//g, "_").replace(/=+$/, "");
    // P19fcG9ydGFsX3Byb21vX2NvZGU9VEVTVF9DT0RF
    ```

2. Put the result into `startapp`:

    ```
    https://t.me/<bot_username>/<app_name>?startapp=P19fcG9ydGFsX3Byb21vX2NvZGU9VEVTVF9DT0RF
    ```

3. Open the link in Telegram. If the Mini App is already open, close it first: otherwise Telegram may restore the running app without reloading it, and the SDK returns the code from the previous launch.
4. Check that the method returns `"TEST_CODE"`.

!!! note
    An unencoded code (`?startapp=TEST_CODE`) is ignored.
