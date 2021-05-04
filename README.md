# KaiAds for Defold
This is an integration of the KaiAds SDK with the Defold game engine. The integration is created as a Defold native extension.

## Installation
To use KaiAds in your Defold project, add the following URL to your game.project dependencies:

https://github.com/defold/extension-kaiads/archive/main.zip

We recommend using a link to a zip file of [a specific release](https://github.com/refold/extension-kaiads/releases).

## Usage
The following functions are available from Lua:

* `kaiads.init(publisher)` - Initialize kaiads 
* `kaiads.set_listener(fn)` - Callback function which will receive ad events
* `kaiads.preload(configuration)` - Preload an ad using the provided JSON encoded Lua table with ad configuration values (see below)
* `kaiads.show()` - Show the ad if it was successfully preloaded (event == kaiads.PRELOAD_OK)

The following ad events are available:

* `PRELOAD_ERROR` - Error when preloading ad
* `PRELOAD_OK` - Ad successfully preloaded
* `SHOW_ERROR` - Error when showing ad
* `AD_DISPLAY` - Ad successfully displayed on device
* `AD_CLICK` - User clicked the ad
* `AD_CLOSE` - User closed the ad

### Configuration
Possible values in the configuration table:

* `app` = Optional, application name, used for reporting, for your own convenience
* `slot` = Optional, ad slot name, used for reporting, for your own convenience
* `container` = Id of HTML div to load banner ad in
* `test` = Optional. Enable test mode. Please set this to 1 when testing the ad, 0 or omitted when in production.

Refer to [KaiAds SDK documentation](https://www.kaiads.com/publishers/sdk.html) for more information on how to configure ads.

### Example
```Lua
local json = require "kaiads.json"

local function on_kaiads_event(self, event, code)
	if event == kaiads.PRELOAD_OK then
		print("KaiAds has successfully preloaded an ad")
		kaiads.show()
	elseif event == kaiads.AD_DISPLAY then
		print("KaiAds is showing an ad")
	elseif event == kaiads.AD_CLOSE then
		print("The user closed the ad!")
	elseif event == kaiads.AD_CLICK then
		print("The user clicked on the ad!")
	else
		print("Something went wrong", code)
	end
end

function init(self)
	if kaiads then
		kaiads.set_listener(on_kaiads_event)
		kaiads.init("2b30c65e-efde-4930-990e-ded207899766")
		local fullscreen_config = {
			app = "mygame",
			slot = "gameover",
		}
		kaiads.preload(json.encode(fullscreen_config))
	end
end
```
