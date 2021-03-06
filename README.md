# GS Grenade Helper
Public repo for GS grenade helper, including files that are streamed live

## Location format
```js
{
	"name": ["T Roof", "Scaffolding Box"], // array of from and to, alternatively a single string
	"description": "Jump on the left box for a good one-way", //optionally, a description can be given
	"weapon": "weapon_smokegrenade", // weapon console name
	"position": [691.63653564453, -1130.1051025391, -127.96875], // origin
	"position_visibility": [-44, 0, 0], // offset to origin for world vischeck, defaults to [0, 0, 0]
	"viewangles": [-1.8710323572159, -136.26739501953], // pitch, yaw
	"duck": true, // true = have to be fully ducked, defaults to false
	"tickrate": 128, // number: all tickrates supported, array: the only tickrates supported
	"approach_accurate": true, // full speed movement during approach, auto-checked by default
	"grenade": {
		"strength": 0.5, // required m_flThrowStrength to autothrow, 1=left, 0.5 = right+left, 0 = right
		"fov": 0.3, // have to be in this fov to autothrow
		"jump": true, // jumpthrow at the end of running
		"run": 12, // run duration in seconds/64
		"run_yaw": 0, // offset to viewangles for move yaw
		"run_speed": true, // hold IN_SPEED (shift) during pre-throw run, defaults to false
		"recovery_yaw": 90, // yaw for movement after throw, only rage aimbot mode. Defaults to run_yaw-180
		"delay": 5 // delay before throwing, useful for getting the max height in a jumpthrow. Defaults to 0
	},
	"destroy": { // a breakable world object has to be destroyed before autothrowing / playback
		"start": [392.701141, -1442.725342, 1936.63842], // trace_line starts from here
		"end": [232.03129134004, -1425.9891813532, 1899.5775623479], // trace_line ends here
		"text": "Break the left window" // text to add if trace_line hit something
	},
	"target": [-19.584310531616, -1810.5485839844, -110.97956085205] // grenade / shot will land here
}
```

movement recordings are saved like this:

```js
{
	"name": ["T Roof", "Scaffolding Box"], // array of from and to, alternatively a single string
	"weapon": "weapon_molotov", // weapon console name
	"position": [691.63653564453, -1130.1051025391, -127.96875], // origin
	"position_visibility": [-44, 0, 0], // offset to origin for world vischeck, defaults to [0, 0, 0]
	"viewangles": [-1.8710323572159, -136.26739501953], // pitch, yaw
	"tickrate": [64, 128], // number: all tickrates supported, array: the only tickrates supported
	"destroy": { // a breakable world object has to be destroyed before autothrowing / playback
		"start": [392.701141, -1442.725342, 1936.63842], // trace_line starts from here
		"end": [232.03129134004, -1425.9891813532, 1899.5775623479], // trace_line ends here
		"text": "Break the left window" // text to add if trace_line hit something
	},
	"movement": {
		"frames": [
			// each frame is encoded as an array. 
			[
				0.1826, // pitch delta, defaults to 0
				0.3512, // yaw delta, defaults to 0
				"WAd" // buttons pressed / released this frame, uppercase = pressed, lowercase = released
				450, // forwardmove, defaults to whatever the buttons would result in
				-450, // sidemove, defaults to whatever the buttons would result in
			],
			// members can be left out from back to front if they are set to their default values
			[
				0.2873,
				0.47
			],
			// if multiple frames would be empty (nothing changed from previous frame), they can be
			// replaced by the number of frames that were left out
			8,
			// the last frame should release all movement keys
			[
				0,
				0,
				"wa"
			]
		]
	}
	"target": [-19.584310531616, -1810.5485839844, -110.97956085205] // you end up here
}
```

## Export / Import format
```js
{
	"name": "sothatwemaybefree.com",
	"description": "Legit grenades from sothatwemaybefree.com", // description, will be shown in menu
	"update_timestamp": 1596507260, // update timestamp, will be shown in menu
	// optional, will overwrite the locally stored url and cause that url to be re-requested
	// use this if you wanna change the URL without breaking everyone's configs
	"url": "https://raw.githubusercontent.com/sapphyrus/helper/master/locations/sothatwemaybefree.json",
	"locations": {
		"de_mirage": [
			// array of locations
		]
	}
}
```

alternatively, there is a version of this where locations are fetched per-map to reduce bandwidth usage

```js
{
	"name": "sothatwemaybefree.com",
	"description": "Legit grenades from sothatwemaybefree.com", // description, will be shown in menu
	"update_timestamp": 1596507260, // update timestamp, will be shown in menu
	// url where to get the map data from, %map% will be replaced with the map name
	// a map will always be fetched when loading. Content-Type is ignored but status has to be 200 OK
	"url_format": "https://raw.githubusercontent.com/sapphyrus/helper/master/maps/%map%",
	"locations": {
		"de_dust2old": "de_dust2" // this allows you to re-map mapnames
	}
}
```
