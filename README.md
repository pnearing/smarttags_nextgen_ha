# SmartThings Find NextGen
This is a spiritual successor to https://github.com/Vedeneb/HA-SmartThings-Find as I really wanted that feature.

Viewing smart tags locations on Home Assistant

Each smart tag shows up as a device with its location, battery and last seen time, and buttons to make it ring or to ask it for its current location.

Phones, tablets, watches and earbuds in your SmartThings Find account show up as devices with a switch to make them ring. It shows whether the device is ringing, also when it was rung from the SmartThings app, and turning it off stops the ring. Their location isn't shown.

For automations there are two actions: **SmartThings Find NextGen: Request location** (`smarttags_nextgen.locate`) asks the targeted tags for their current location, like their refresh location button, and **SmartThings Find NextGen: Refresh** (`smarttags_nextgen.refresh`) updates all devices without waiting for the update interval.

I only have 1 smart tag so I haven't tested it with more than 1, it should work as I dynamically get the list but it might be broken on certain conditions.

<!-- ha-marketplace:exclude-start -->
## Installation

### Home Assistant 2026.11 and later

SmartThings Find NextGen is a custom integration. Install it from **Settings → Marketplace**, not **Settings → Apps**. HACS is built into Home Assistant as Marketplace starting with 2026.11.

[![Open SmartThings Find NextGen in the Home Assistant Marketplace](https://my.home-assistant.io/badges/marketplace_repository.svg)](https://my.home-assistant.io/redirect/marketplace_repository/?owner=pnearing&repository=smarttags_nextgen_ha&category=integration)

1. Open **Settings → Marketplace** and select **Add from link**.
2. Enter `https://github.com/pnearing/smarttags_nextgen_ha`. Connect GitHub if prompted. If asked for a type, select **Integration**.
3. Open **SmartThings Find NextGen** in Marketplace and select **Install**. Restart Home Assistant if it asks you to, then follow the setup instructions below.

If HACS already managed this integration before you upgraded, Home Assistant migrates that installation to Marketplace. Look under **Marketplace → Installed** first. You don't need to remove or re-add your Samsung account or devices.

### Home Assistant 2026.10 and earlier

In **HACS → ⋮ → Custom repositories**, add `https://github.com/pnearing/smarttags_nextgen_ha` as an **Integration**. Download **SmartThings Find NextGen**, then restart Home Assistant.

### Manual installation

1. Download or clone this repository. Copy its `custom_components/smarttags_nextgen` folder into the `custom_components` directory in your Home Assistant configuration folder. Create `custom_components` if it doesn't exist.
2. Confirm that `custom_components/smarttags_nextgen/manifest.json` is present in your Home Assistant configuration folder, then restart Home Assistant.

<!-- ha-marketplace:exclude-end -->

## Setup Instructions

1. Go to **Settings → Devices & services → Add integration**.
2. Search for **SmartThings Find NextGen**.
3. Choose one of the two sign-in methods below.

### Option 1: Sign in with Samsung account (sessions renew automatically)

1. Open the Developer Tools of your browser (F12) and switch to the **Console** tab.
2. Open the sign-in link shown by Home Assistant in the same tab and sign in to your Samsung account.
3. At the end, the browser tries to open an address starting with `ms-app://` and fails. Copy that complete address (Chrome and Edge show it in the Console as *Failed to launch 'ms-app://…'*, otherwise look for it in the **Network** tab) and paste it into Home Assistant.

Home Assistant keeps a Samsung account token and uses it to create a new SmartThings Find session whenever the current one expires, so you only need to sign in again after logging out of your Samsung account or changing its security settings.

**Treat this token like your Samsung password:** anyone with access to your Home Assistant configuration (or its backups) can use it to access your Samsung account. The sign-in is based on the reverse-engineered protocol documented by [samsung-re-find](https://github.com/charlesbel/samsung-re-find) and [uTag](https://github.com/KieronQuinn/uTag/wiki/Authentication); Samsung can change or block it at any time.

### Option 2: Enter a JSESSIONID

1. Visit https://smartthingsfind.samsung.com/ and log in with your Samsung account.
2. Open Developer Tools in your browser.
3. Copy the JSESSIONID value. Note to copy the one from smartthingsfind.samsung.com (you might have another from another domain).
4. Enter your JSESSIONID into Home Assistant.

The session expires after a while; Home Assistant then asks you for a new JSESSIONID.

When a session expires, or Samsung no longer accepts the account token, a notification with the id `smarttags_nextgen_session_expired` is shown until the integration can update again. Automations can trigger on it with the **Persistent notification** trigger, e.g. to send it to your phone.

You can switch between both options, or change the region, with **Reconfigure** in the integration's menu. The update interval can be changed in the integration's options. There you can also choose to ask the tags for their current location on every update, as their refresh location button does. Without it, the location a tag last reported is shown. Nearby Galaxy devices then look for the tags on every update, which uses more of their battery.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributions

Contributions are welcome! Feel free to submit pull requests to help improve this integration.

## Support

For integration bugs, see the [upstream issue tracker](https://github.com/saarglobin/smarttags_nextgen_ha/issues).

## Roadmap

- Maybe more comfortable login

## Disclaimer

This is a third-party integration and is not affiliated with or endorsed by Samsung or SmartThings.
