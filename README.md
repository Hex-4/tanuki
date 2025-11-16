# tanuki

hi! this is a little Tauri rewrite of [Nook](https://github.com/mn6/nook-desktop/tree/main), a small desktop app that plays the hourly music from Animal Crossing on your computer. The original is currently unmaintained and a little buggy on my Wayland system, so I took matters into my own hands :)

tanuki is far from complete, but it currently includes most of the features that I use daily:

- Plays music based on actual system time
- Includes sets from Population Growing, Wild World, New Leaf, and New Horizons, as well as rainy and snowy variants
- "K.K. Slider" game option that plays K.K. tracks sequentially, with a button to skip the current song.
- Optional rain ambience
- Play/pause rain or music + volume control
- Minimize to system tray

Features that are *not* implemented:

- Town tunes
- K.K. playlist selection
- Offline support
- Rain type selection
- K.K. on Saturday nights
- Localization

## how to use

tanuki has not been extensively tested! please create an issue if you run into anything ;)

download from the [latest release](https://github.com/Hex-4/tanuki/releases/latest)! we've got a pretty wide range of options for windows, linux, and mac.

this was developed on a wayland linux system so I cannot promise compatibility with other OSes.

## credits

tanuki is a [Tauri](https://tauri.app/) app, with Bun, Svelte and Tailwind used for the frontend. Many things were borrowed from Nook, such as the CDN (for audio files) and the lists of games.

Icons were taken from the [Animal Crossing UI Kit](https://www.figma.com/community/file/1155196388062943961) and [Hack Club Icons](https://icons.hackclub.com/). Thanks!



