<template>
  <f7-app v-bind="f7params">
    <f7-view main class="safe-areas" url="/"></f7-view>
  </f7-app>
</template>
<script>
import {getDevice} from 'framework7/lite-bundle';
import routes from './js/routes.js';
import store from './js/store';

export default {
  setup() {
    const device = getDevice();
    // Framework7 Parameters
    const f7params = {
      name: 'runway_strip', // App name
      theme: 'auto', // Automatic theme detection
      id: 'io.framework7.runway_strip', // App bundle ID
      // App store
      store: store,
      // App routes
      routes: routes,
      // Input settings
      input: {
        scrollIntoViewOnFocus: device.cordova && !device.electron,
        scrollIntoViewCentered: device.cordova && !device.electron,
      },
      // Cordova Statusbar settings
      statusbar: {
        iosOverlaysWebView: true,
        androidOverlaysWebView: false,
      },
    };

    return {
      f7params,
    }
  }
}
</script>