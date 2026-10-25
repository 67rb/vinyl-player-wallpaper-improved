# Native wallpaper review — 2026-10-11

## Implemented

- Downpour: 720 fast falling streaks plus 260 flowing glass drops. Previously it had 138 slow glass drops only. Storm uses 1,050 falling streaks plus 360 glass drops; light rain uses 120 streaks plus 65 glass drops.
- The native app drives the canvas at up to 24 fps through a common-mode timer. Rendering no longer stops at `document.hidden`. An activity token prevents App Nap during a selected animated desktop scene while allowing ordinary idle system sleep. Spotify polling and permission state cannot stop the animation timer.
- Real filmed fire replaces the static flame region. The media loops locally, uses a correctly proportioned center crop, and has playback recovery if the media stalls.
- Added separate overhead and angled tonearm assets with a simpler pivot. Placement uses explicit rectangle geometry rather than stretching the previous detailed gimbal sprite.
- Revised all four new scene interiors toward less saturated light, fewer decorative items, and quieter material textures. The three rain views use real photographed wet glass and greenery; the fire uses real footage. Interiors remain generated; see `asset-provenance.md`.

## Evidence

- Native release build succeeded after the Swift changes and after integrating the revised resources. JavaScript syntax checks passed for `atmosphere.js`, `wallpaper.js`, and `settings.js`. The canonical app at `/Users/robeberry/Applications/Vinyl Wallpaper.app` passes strict deep signature verification and its Info.plist validates. The build signs outside FileProvider-managed storage; the project copy remains available as an artifact.
- `rain-duration-review.json` contains 37 real native-desktop samples across 360 seconds. Every consecutive sample advances the frame counter, remains in scene 5, and reports the native driver. First frame: 636; final frame: 8,528. The app was rendering with room sound off and Spotify paused.
- A later diagnostic after that run reported frame 11,509, 521 seconds of rendering, and `hidden: true`, with the same 260 drops and 720 streaks. This verifies progress beyond the reported five-minute stopping point and while the document was hidden.
- The native downpour preview was visually inspected before the final asset integration. Stronger streaks were visible, and the sleeve/record/history loaded correctly.
- Fire media is actual H.264 footage, 24 fps, locally bundled. Its duration and decoded frame differences are checked separately; this alone does not prove the final native canvas is displaying moving flames.

## Final native review

The Mac temporarily locked before reopening the rebuilt app, then became accessible again. The clean app is running from Applications.

- Fireside: real decoded flames visible inside the firebox with correct crop and no static flame underneath. `fire-playback-review.json` records 60 seconds and four completed video loops. Every sample reports ready state 4, active playback, and no media error. Native screenshots `review/fire-before.jpg` and `review/fireside.jpg` show distinct flame shapes; `review/fire-motion-evidence.json` quantifies changes within the firebox itself.
- Rain: `review/downpour.jpg` shows strong moving streaks over photographed wet glass, with the simplified room and new angled arm. `review/soft-rain-settings.jpg` and `review/thunderstorm-settings.jpg` confirm distinct light and storm presentation. The six-minute native runtime check proves the reported five-minute stopping point is passed; later diagnostics also continued while hidden.
- Tonearm: `review/overhead.jpg` confirms an overhead pivot without the former bulky gimbal and a cartridge on the grooves. The angled arm is visible on the record in the rain, fireside, afternoon, after-hours, and listening-room captures.
- All eight scene selections were checked in the native app. The current artwork, record, and previous-played artwork rendered across transitions; Spotify playback updates continued during the review. Gallery thumbnails now use the same photographic window and filmed-fire poster as the selected scene.
- The less glossy lighting, fewer decorations, genuine window photograph, and moving real fire were reviewed against the prior new-scene assets. Generated interiors are explicitly identified in `asset-provenance.md`.

Result: the requested heavier persistent rain, moving real fire, revised new wallpapers, and corrected tonearm are implemented and checked in the native app. The canonical installed app's signature and bundled resources were verified.


## October 11 performance revision

The desktop renderer has changed since the previous duration review: rain is now implemented in `RainView.swift` using repeating native Core Animation tiles. Its motion is handled by the system compositor; canvas frame counters are zero for this route by design. The app is now a regular Dock application with its own icon, app menus, reopen behavior, and Quit shortcut.

A background process sample before this revision showed the host at 17.6% CPU; the first native compositor sample showed 1.6%, with all three wallpaper WebContent processes at 0.6% or less. These are process samples, not a controlled benchmark or a measurement of WindowServer GPU cost. The build and installed app signature passed. Native accessibility exposes the standard app window and app menu. Screen capture was unavailable during this revision, so visible smoothness and the new native rain geometry still need a visual pass. Previous screenshots and long-duration recordings describe the earlier renderer.

Final packaging note: the signed 1.1 bundle is installed at `~/Applications/Vinyl Wallpaper.app`. The last attempt to launch and exercise its menus was blocked explicitly by the locked Mac. Unlocking and opening that bundle is required for the final visual check. A later runtime sample showed native layer offsets changing over 11 seconds; host samples varied from 0.3% to 21.8% during Spotify artwork updates. The initial reduction is therefore a background observation, not a bound on peak CPU use.

## Follow-up: Screen Time blocker identified

After the Mac was unlocked, the native collection window could be captured and its room, album artwork, and gallery appeared correctly. Quit was exercised through the standard menu and the app exited. Finder then revealed the cause of the remaining foreground interaction problem: `~/Applications/Vinyl Wallpaper.app` has a “Disabled by Screen Time” indicator. The process renders in the background, but it cannot become the active app. Screen Time was not changed. The native preview now uses the same Core Animation rain renderer as the desktop. Menu validation uses normal AppKit action routing; temporary manual enabling workarounds were removed. The final foreground interaction review remains pending the user allowing the app in Screen Time.


## October 11 rain direction correction

The user reported upward rain in the native renderer. Reversed the repeating rain belt animation from 0 → 535 to 535 → 0, preserving the two-tile seamless loop and existing speeds. Rebuilt, signature-verified, installed, and restarted the canonical app. Opened its native Downpour preview and captured consecutive frames (`review/rain-direction-c.jpg`, `review/rain-direction-d.jpg`, `review/rain-direction-e.jpg`). Correlating successive frame differences within the window region gave the strongest match at a positive 24-pixel vertical displacement, confirming downward movement in screenshot coordinates. This direction correction applies to all three native rain scenes and their native preview.


## October 11 white flashes and memory revision — version 1.2

The user reported occasional white wallpaper and excessive RAM. The old host sampled at 355.5 MB physical footprint (466.7 MB peak), with several WebContent processes and a GPU helper. Its session contained 5,058,955 bytes, and the full state/artwork was passed to web views on every Spotify poll. Logs showed repeated WebContent loads and process turnover; they do not establish a unique cause for every reported white frame.

Desktop windows and the full-size preview now use SceneView native layers. There is no desktop WKWebView, heartbeat JavaScript, or desktop page reload. Screen-change notifications reuse existing windows when display frames have not changed. Rain uses five shared cached tile images, preserving density and downward motion. Fireside uses AVQueuePlayer/AVPlayerLooper and AVPlayerLayer, released when leaving the scene. Ambient audio is native. Settings remains WebKit-based, uses reduced gallery thumbnails, and releases its view and message handler when closed. Unchanged playback state is skipped before serialization; downloaded covers are decoded off the main thread, resized to 512 pixels and JPEG-encoded, and stored in bounded caches. Existing artwork is migrated once without repeatedly recompressing JPEGs.

The release build and strict signature verification succeeded, and version 1.2 is installed and running from ~/Applications/Vinyl Wallpaper.app. Native Downpour and Fireside previews were inspected; screenshot evidence is in review/native-scene-rain-*.jpg and review/native-scene-fire-*.jpg. Consecutive rain frame differences match a positive 66-pixel vertical displacement (downward). Two fireplace captures show mean absolute pixel change 13.78 in the firebox. Closing preview and collection windows succeeds. With Downpour restored and both windows closed, diagnostics report desktopWebViews: 0 and settingsLoaded: false, the rain offsets advance, no WebContent processes remain in the process sample, and the host physical footprint measures 145.2 MB (203.4 MB peak). These are sampled host measurements, not a controlled total-memory benchmark. The captures show intact scenes; the removal of the desktop page/reload path addresses the reported white flash mechanism, but this short review cannot establish that every intermittent display fault is eliminated.
