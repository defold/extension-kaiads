# KaiAds for Defold
This is an integration of the KaiAds SDK with the Defold gane engine. The integration is created a Defold native extension.

## Installation
To use KaiAds in your Defold project, add the following URL to your game.project dependencies:

https://github.com/defold/extension-kaiads/archive/master.zip

We recommend using a link to a zip file of [a specific release](https://github.com/refold/extension-kaiads/releases).

## Usage

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
