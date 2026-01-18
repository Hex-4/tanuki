# tanuki

hi! this is a little Tauri rewrite of [Nook](https://github.com/mn6/nook-desktop/tree/main), a small desktop app that plays the hourly music from Animal Crossing on your computer. The original is currently unmaintained and a little buggy on my Wayland system, so I took matters into my own hands :)

tanuki is far from complete, but it currently includes most of the features that I use daily:

- Plays music based on actual system time
- Includes sets from Population Growing, Wild World, New Leaf, and New Horizons, as well as rainy and snowy variants
- "K.K. Slider" game option that plays K.K. tracks sequentially, with a button to skip the current song.
- Optional rain ambience
- Play/pause rain or music + volume control
- Minimize to system tray

Features that are _not_ implemented:

- Town tunes
- K.K. playlist selection
- Offline support
- Rain type selection
- K.K. on Saturday nights
- Localization

## how to use

tanuki has not been extensively tested! please create an issue if you run into anything ;)

download from the [latest release](https://github.com/Hex-4/tanuki/releases/latest)! we've got a pretty wide range of options for windows, linux, and mac.

if you use arch (btw), you can get Tanuki from the [AUR](https://aur.archlinux.org/packages/tanuki-bin).

on windows and mac you will probably need to dismiss the code-signing warning:

- windows: "Windows protected your PC" -> More info -> Run anyways
- mac: after moving tanuki to your Applications folder, right click the app -> Open -> Open

this appears due to me not having the funding to purchase a code-signing certificate. if you're skeptical, note that the downloads are [built from source](https://github.com/Hex-4/tanuki/actions) by GHA, then directly uploaded by the workflow.

this was developed on a wayland linux system so I cannot promise compatibility with other OSes.

## credits

tanuki is a [Tauri](https://tauri.app/) app, with Bun, Svelte and Tailwind used for the frontend. Many things were borrowed from Nook, such as the original audio files (update: now hosted on my own CDN) and the lists of games.

Icons were taken from the [Animal Crossing UI Kit](https://www.figma.com/community/file/1155196388062943961) and [Hack Club Icons](https://icons.hackclub.com/). Thanks!
