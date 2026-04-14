# **Ad Lifecycle Callbacks**

The SDK provides two callback properties for tracking the advertisement lifecycle:

* `onAdStart` — Called when the advertisement begins showing.  
* `onAdEnd` — Called when the advertisement has finished.

### **onAdStart**

* The callback is called before the ad display begins.  
* **Signature:** `PortalSDK.onAdStart = () => void`
* **Parameters:** None.  
* **Usage Example:**
=== "Unity"
	```C#
    PortalSDK.SetOnAdStart(OnAdStart);

    [MonoPInvokeCallback(typeof(object))]
    private static void OnAdStart(bool _)
    {
    Debug.Log($"[PortalSDK] OnAdStart");
    }
	```

=== "Defold"
	```LUA
    print("Test: SetOnAdStart()")
    portalsdk.set_on_ad_start(function(self, success)
    print("OnAdStart: " .. tostring(success))
    end)
	```

=== "JavaScript"
    ```js
    PortalSDK.onAdStart = function() {  
        console.log('Ad started');  
        pauseGame();  
        muteSound();
    };
    ```

### **onAdEnd**

* The callback is called after the ad finishes (whether successful or not).  
* **Signature:** `PortalSDK.onAdEnd = (result: boolean) => void`
* **Parameters:**  
  * result (boolean) — true if the ad was shown successfully, false if the ad was not shown (e.g., `error`, `cooldown`, `ads_free`, etc.).  
* **Usage Example:**
=== "Unity"
	```C#
    PortalSDK.SetOnAdEnd(OnAdEnd);

    [MonoPInvokeCallback(typeof(object))]
    private static void OnAdEnd(bool success)
    {
    Debug.Log($"[PortalSDK] OnAdEnd {success}");
    }
	```

=== "Defold"
	```LUA
    print("Test: SetOnAdEnd()")
    portalsdk.set_on_ad_end(function(self, success)
    print("OnAdEnd: " .. tostring(success))
    end)
	```

=== "JavaScript"
    ```js
    PortalSDK.onAdEnd = function(success) {  
        console.log('Ad ended, success:', success);  
        resumeGame();  
        unmuteSound();
        if (success) {  
            giveRewardToPlayer();  
        }  
    };
    ```

### **Disabling Callbacks**

To disable a callback, set its value to null:

=== "Unity"
	```C#
    PortalSDK.ClearOnAdStart();
    PortalSDK.ClearOnAdEnd();
	```

=== "Defold"
	```LUA
    portalsdk.clear_on_ad_start()
    portalsdk.clear_on_ad_end()
	```

=== "JavaScript"
    ```js
    PortalSDK.onAdStart = null;  
    PortalSDK.onAdEnd = null;
    ```

### **Notes**

* Callbacks are triggered for both methods: `requestAd()` and `requestRewardAd()`.  
* `onAdEnd` is called in all completion scenarios (`success`, `error`, `cooldown`, `ads_free`).  
* Callbacks are synchronous — they do not block the execution of the ad display logic.  
* By default, both properties are set to null.