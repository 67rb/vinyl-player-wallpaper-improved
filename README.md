# Vinyl Wallpaper for Mac

A native macOS wallpaper app that mirrors the music currently playing in Spotify. Its vinyl spins during playback, the current cover sits on the record stand, and previous tracks collect on the right. Four original listening rooms plus light rain, heavy rain, a thunderstorm, and a fireside room are included.

## Install and run

Open `~/Applications/Vinyl Wallpaper.app`. The build verifies this copy outside the synced workspace to avoid FileProvider metadata invalidating its signature. A workspace copy is also saved at `../Vinyl Wallpaper.app`. The app appears in the Dock and the menu bar. Click its Dock icon to reopen the collection window; Command-comma opens settings and Command-Q quits. Closing the collection window leaves the wallpaper running. Choose **Wallpapers & Spotify…** to select a scene, preview the wallpaper, reconnect Spotify, or set optional room sounds. When macOS first asks, allow Vinyl Wallpaper to read Spotify playback. Playback stays on Spotify; the wallpaper only reads its track and artwork.

The desktop wallpaper ignores mouse clicks so Finder remains usable. Each display gets its own matching wallpaper. Scene choice, Spotify playback history, and room sound preference are saved locally.

The desktop and full-size preview use native Core Animation layers for the room, record, album artwork, tonearm, history, and rain. They have no WebKit renderer or page reload. Fire plays through a native AVPlayerLayer only while Fireside is selected. Rain tiles and artwork are shared between displays with bounded caches. The collection window uses smaller gallery thumbnails and releases its web view when closed. Artwork is resized to 512 pixels and unchanged Spotify state is not resent. Optional rain, fire, and thunder audio plays natively. A one-second timer schedules thunder and records diagnostics every ten seconds; it does not draw animation frames.

The rain window uses a real photograph and Fireside uses locally bundled footage of actual flames; room interiors and tonearm assets remain generated. Credits and asset details are in `asset-provenance.md`, and review evidence is in `live-wallpaper-review.md`.

## Build

Run `./build.sh` from this directory. Build playback logic checks with `./check-playback.sh`.
