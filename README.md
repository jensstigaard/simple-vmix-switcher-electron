# simple-vmix-switcher-electron

[![Simple vMix Switcher Electron](https://img.shields.io/github/v/release/jensstigaard/simple-vmix-switcher-electron.svg)](../../releases)
[![Simple vMix Switcher Electron](https://img.shields.io/github/downloads/jensstigaard/simple-vmix-switcher-electron/total.svg)](../../releases)
[![Sponsor me - Simple vMix Switcher Electron](https://img.shields.io/badge/paypal-donate-brightgreen.svg)](https://paypal.me/stigaard)

Simple vMix switcher app built with [ElectronJS](https://electronjs.org). ElectronJS is a cross-platform framework allowing the app to be built for each Windows, Mac or Linux. 

The app is oriented for touch use.

You are free to clone the repository to develop your own app based in this code.

![Simple vMix Switcher Electron](./readme_assets/overview_030.png "Application overview")

## Downloads
### Latest version (1.2.0)
[For Windows](../../releases/download/v1.2.0/Simple.vMix.Switcher.Electron.Setup.1.2.0.exe)

[For MacOS](../../releases/download/v1.2.0/Simple.vMix.Switcher.Electron-1.2.0.dmg)


See the [Releases](../../releases) tab for a direct download of the app for Mac and Windows.

## Feature summary
 - Traditional preview and program row including 'tally'
 - Dynamic number of inputs visible based on window width
 - Shows badges (tally) on input in overlay channel preview/program
 - Long touch on input in program row enables input to be put into an overlay channel
 - Shortcut: `Ctrl+Shift+P` (Win+Linux) / `Cmd+Alt+P` (Mac) to swap Preview and Program row order
 - Transitions row: Cut and Quick Play + 4 defined transitions in vMix
 - Transition progress (seen between Program and Preview row)
 - vMix host connection
   * Status dot
   * Easy change of host
   * Auto-complete (Saves list of previous connected hosts)


## Known issues
When running in development mode you can experience loss of connection to the vMix instance after some minutes.

## Project setup
### Install dependencies (based on package.json)
```
yarn
```

### Compiles and hot-reloads for development
```
yarn electron:serve
```

### Compiles and minifies for production
```
yarn electron:build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
