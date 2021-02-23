# Quasar App (vue-wether-app)

This is a Vuejs-Quasar weather app that work on iOS/Android, Mac and Windows machines. 

## Install the dependencies
```bash
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```


### Build the app for production
```bash
quasar build
```
### Windows
   quasar dev -m electron
### Customize the configuration
See [Configuring quasar.conf.js](https://quasar.dev/quasar-cli/quasar-conf-js).

### build electron for Windows
  ``-- quasar config
  electron: {
      bundler: 'packager', // 'packager' or 'builder'

      packager: {
              platform: 'win32'``

quasar dev -m electron
quasar dev -m android

### Install cordova
npm install -g cordova
### Cordova Plugin Installation for iOS
cordova plugin add cordova-plugin-geolocation
## Android
Install the plugin 
```cordova plugin add cordova-plugin-geolocation --variable GPS_REQUIRED="false"```
 