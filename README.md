# Cordova Plugin: Android WiFi Manager

Cordova plugin for accessing Android's `WifiManager`.

## Installation

Install the plugin using Cordova CLI:
```sh
cordova plugin add git+https://github.com/guinhx/cordova-plugin-android-wifi-manager.git
```

## Usage

Access the plugin after the device is ready. The plugin is exposed at `window.cordova.plugins.WifiManager`.

```javascript
var WifiManager = cordova.plugins.WifiManager;

// Listen for WiFi state changes
WifiManager.onwifistatechanged = function (data) {
  console.log(data.previousWifiState, '->', data.wifiState);
};

// Turn on WiFi
WifiManager.setWifiEnabled(true, function (err, success) {
  console.log(err, success);
});
```

## API Reference

All exposed methods and events have corresponding [WifiManager](https://developer.android.com/reference/android/net/wifi/WifiManager.html) counterparts. Methods accept an optional callback as the last argument, receiving either an error object or the returned value.

### Network Configuration

#### `addNetwork(wifiConfiguration, callback(err, netId))`
Adds a new network to the configured networks list.

Example *WifiConfiguration* object:
```javascript
{
  SSID: 'my-ssid',
  BSSID: 'any',
  preSharedKey: 'psk',
  hiddenSSID: false,
  allowedKeyManagement: { WPA_PSK: true },
  status: 'ENABLED',
  networkId: 1,
  allowedAuthAlgorithms: { OPEN: true },
  allowedProtocols: { WPA: true },
  allowedPairwiseCiphers: { CCMP: true },
  allowedGroupCiphers: { TKIP: true }
}
```
Possible values for `status`: `ENABLED`, `DISABLED`, `CURRENT`.

#### `disableNetwork(netId, callback(err, success))`
Disables a configured network.

#### `enableNetwork(netId, attemptConnect, callback(err, success))`
Allows a previously configured network to be used.

#### `getConfiguredNetworks(callback(err, wifiConfigurations))`
Retrieves a list of all configured networks.

#### `removeNetwork(netId, callback(err, success))`
Removes a configured network.

#### `saveConfiguration(callback(err, success))`
Saves the current list of configured networks.

### Connection Management

#### `disconnect(callback(err, success))`
Disconnects from the currently active access point.

#### `reassociate(callback(err, success))`
Reassociates with the currently active network.

#### `reconnect(callback(err, success))`
Reconnects if disconnected.

#### `setWifiEnabled(enabled, callback(err, success))`
Enables or disables WiFi.

#### `getWifiState(callback(err, wifiState))`
Retrieves the WiFi state. Possible values:
- `DISABLED`
- `DISABLING`
- `ENABLED`
- `ENABLING`
- `UNKNOWN`

### Network Information

#### `getConnectionInfo(callback(err, wifiInfo))`
Gets the current WiFi connection details.

Example *WifiInfo* object:
```javascript
{
  SSID: 'my-ssid',
  BSSID: 'any',
  ipAddress: 2130706433,
  macAddress: '00:14:22:01:23:45',
  linkSpeed: 2300,
  rssi: -15,
  supplicantState: 'COMPLETED',
  frequency: 2412,
  networkId: 1,
  hiddenSSID: false
}
```

#### `getDhcpInfo(callback(err, dhcpInfo))`
Retrieves the last successful DHCP request's assigned addresses.

Example *DhcpInfo* object:
```javascript
{
  dns1: 19216811,
  dns2: 19216812,
  gateway: 19216810,
  ipAddress: 19216813,
  leaseDuration: 3600,
  netmask: 2552552550,
  serverAddress: 19216814
}
```

#### `getScanResults(callback(err, scanResults))`
Gets the results from the latest WiFi scan.

### Events

The plugin emits events for various WiFi state changes.

#### `onnetworkstatechanged({ networkInfo, BSSID, wifiInfo })`
Triggered when WiFi connectivity changes.

#### `onwifistatechanged({ wifiState, previousWifiState })`
Triggered when the WiFi state changes.

#### `onrssichanged({ RSSI })`
Triggered when signal strength changes.

#### `onscanresultsavailable({ resultsUpdated })`
Triggered when a WiFi scan is completed.

#### `onevent(name, data)`
Called for all events with the event name and extra data.

#### `onerror`
Called on internal errors.

### Hotspot Management (Experimental)

Since the Android API does not expose hotspot modification, private and undocumented methods are used. Functionality may be limited and version-dependent.

#### `getWifiApConfiguration(callback(err, wifiConfiguration))`
Retrieves the current hotspot configuration.

#### `setWifiApEnabled(wifiConfiguration, enabled, callback(err, success))`
Enables or disables the WiFi hotspot with the given configuration.

#### `onwifiapstatechanged({ wifiApState, previousWifiApState })`
Triggered when the WiFi hotspot state changes.

## Notes
- This plugin interacts with Android's native `WifiManager`, so behavior may vary by Android version and device manufacturer.

## License
This plugin is released under the MIT License.