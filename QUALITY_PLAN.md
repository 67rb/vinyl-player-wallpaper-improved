# Vinyl Wallpaper quality pass

The user asked to substantially improve the app and fix its issues. This preserves the intended product: a Mac app with eight vinyl rooms, live Spotify artwork and playback, a stack of played records, rain/storm/fire scenes, optional room sounds, and a usable collection window. “100x” is treated as a request for a comprehensive quality improvement, not an invented quantitative benchmark.

## Requirements and evidence

1. **One accurate rendering path.** Collection, full preview and desktop use the same geometry, assets, artwork and lighting. Review all eight rooms. Rain stays behind foreground objects, falls downward and continues over time. Fire actually plays and loops.
2. **Bounded resource use.** No web renderer is necessary. Images and network requests are bounded, closed views release their resources, inactive previews pause, and screen sleep suspends animations/audio. Measure the installed app after opening and closing windows and changing scenes.
3. **Reliable music state.** Pause/resume preserves record position; track changes update artwork/history correctly; paused browsing does not add unplayed records; missing Spotify, denied access and failed artwork have understandable recovery. Exercise model checks and real app state without changing the user's Spotify playback.
4. **Usable controls.** Native collection, eight room selections, enabled toggle, sound/volume, preview, menu/Dock reopen and keyboard navigation work. History clearing can be undone. No blank loading page or inactive navigation button.
5. **Accessibility and comfort.** Controls have useful names and focus; readable labels; preview identifies the scene and track; reduced motion and lightning controls are available and respect macOS preferences. Do not claim full accessibility compliance from screenshots.
6. **Recovery and persistence.** Settings and played history survive restart. Display changes reconcile correctly. Failed media can recover without replacing the whole scene with a blank page. Every successful release is built, signed, installed and launched from the canonical Applications path.
7. **Evidence and limitations.** Save current captures, checks, runtime samples and a concise change report. Mark remaining unverified scenarios honestly; keep the goal active while required work remains.

## Initial audit, current run

1. **Open collection — functional.** The app menu reopens the window. A blank dark loading window appears while WebKit starts, followed by the collection. `review/quality-audit/01-collection-before.jpg` is the accepted loaded state.
2. **Choose room — functional, inconsistent preview.** Eight selections work and the selected card is clear. The gallery and hero use separate assets and a web renderer. Labels and supporting controls are small. `02-selection-before.jpg`.
3. **Full preview — functional, incomplete fidelity/accessibility.** After Hours renders, but its current cover is much brighter than the hero; preview exposes no scene description in accessibility. Native rain was also observed over the sleeve in the current installed renderer. `03-preview-before.jpg`.
4. **Playback and ambience — needs improvement.** Controls sit below the main view, the sound checkbox remains available in silent rooms without an explanation, and Clear has no undo. No reduced-motion or lightning control. `04-controls-before.jpg`.

The screenshots establish visible UI and flow behavior. Source inspection supplies the separate-renderer, lifetime and recovery findings; screenshots alone do not establish performance or full accessibility compliance.
