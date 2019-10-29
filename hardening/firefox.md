## Firefox
+ Add-ons to be installed:
```
NoScript
uBlock Origin
HTTPS Everywhere
Decentraleyes
Privacy Badger
Firefox Multi-Account Containers
```

+ Set a secure/private baseline for the user profile. In Linux, it should be located at `~/.mozilla/firefox/XXXXXXXX.your_profile_name/`. After backing up bookmarks, extension settings etc. empty the directory and copy the preferences below to `user.js`:

```javascript
user_pref("app.normandy.api_url", "");
user_pref("app.normandy.enabled", false);
user_pref("app.shield.optoutstudies.enabled", false);
user_pref("beacon.enabled", false);
user_pref("browser.cache.offline.enable", false);
user_pref("browser.cache.disk.enable", false);
user_pref("browser.disableResetPrompt", true);
user_pref("browser.fixup.alternate.enabled", false);
user_pref("browser.formfill.enable", false);
user_pref("browser.newtab.preload", false);
user_pref("browser.newtabpage.enabled", false);
user_pref("browser.newtabpage.enhanced", false);
user_pref("browser.newtabpage.introShown", true);
user_pref("browser.search.suggest.enabled", false);
user_pref("browser.selfsupport.url", "");
user_pref("browser.send_pings", false);
user_pref("browser.sessionstore.privacy_level", 2);
user_pref("browser.shell.checkDefaultBrowser", false);
user_pref("browser.startup.homepage_override.mstone", "ignore");
user_pref("browser.urlbar.speculativeConnect.enabled", false);
user_pref("browser.urlbar.trimURLs", false);
user_pref("datareporting.healthreport.service.enabled", false);
user_pref("datareporting.healthreport.uploadEnabled", false);
user_pref("datareporting.policy.dataSubmissionEnabled", false);
user_pref("dom.battery.enabled", false);
user_pref("dom.webaudio.enabled", false);
user_pref("experiments.activeExperiment", false);
user_pref("experiments.enabled", false);
user_pref("experiments.manifest.uri", "");
user_pref("experiments.supported", false);
user_pref("extensions.getAddons.cache.enabled", false);
user_pref("extensions.greasemonkey.stats.optedin", false);
user_pref("extensions.greasemonkey.stats.url", "");
user_pref("extensions.shield-recipe-client.api_url", "");
user_pref("extensions.shield-recipe-client.enabled", false);
user_pref("media.autoplay.default", 1);
user_pref("media.autoplay.enabled", false);
user_pref("media.navigator.enabled", false);
user_pref("media.peerconnection.enabled", false);
user_pref("media.video_stats.enabled", false);
user_pref("network.IDN_show_punycode", true);
user_pref("network.allow-experiments", false);
user_pref("network.cookie.cookieBehavior", 1);
user_pref("network.cookie.lifetimePolicy", 2);
user_pref("network.dns.disablePrefetch", true);
user_pref("network.http.referer.XOriginPolicy", 1);
user_pref("network.http.speculative-parallel-limit", "0");
user_pref("network.prefetch-next", false);
user_pref("privacy.donottrackheader.enabled", true);
user_pref("privacy.donottrackheader.value", 1);
user_pref("privacy.firstparty.isolate", true);
user_pref("privacy.resistFingerprinting", true);
user_pref("privacy.trackingprotection.enabled", true);
user_pref("privacy.trackingprotection.pbmode.enabled", true);
user_pref("privacy.trackingprotection.fingerprinting.enabled", true);
user_pref("privacy.trackingprotection.cryptomining.enabled", true);
user_pref("privacy.usercontext.about_newtab_segregation.enabled", true);
user_pref("privacy.clearOnShutdown.cache", true);
user_pref("privacy.clearOnShutdown.cookies", true);
user_pref("privacy.clearOnShutdown.downloads", true);
user_pref("privacy.clearOnShutdown.formdata", true);
user_pref("privacy.clearOnShutdown.history", true);
user_pref("privacy.clearOnShutdown.offlineApps", true);
user_pref("privacy.clearOnShutdown.sessions", true);
user_pref("signon.autofillForms", false);
user_pref("signon.rememberSignons", false);
user_pref("toolkit.telemetry.archive.enabled", false);
user_pref("toolkit.telemetry.bhrPing.enabled", false);
user_pref("toolkit.telemetry.cachedClientID", "");
user_pref("toolkit.telemetry.enabled", false);
user_pref("toolkit.telemetry.firstShutdownPing.enabled", false);
user_pref("toolkit.telemetry.hybridContent.enabled", false);
user_pref("toolkit.telemetry.newProfilePing.enabled", false);
user_pref("toolkit.telemetry.prompted", 2);
user_pref("toolkit.telemetry.rejected", true);
user_pref("toolkit.telemetry.reportingpolicy.firstRun", false);
user_pref("toolkit.telemetry.server", "");
user_pref("toolkit.telemetry.shutdownPingSender.enabled", false);
user_pref("toolkit.telemetry.unified", false);
user_pref("toolkit.telemetry.unifiedIsOptIn", false);
user_pref("toolkit.telemetry.updatePing.enabled", false);
user_pref("webgl.disabled", true);
user_pref("webgl.renderer-string-override", " ");
user_pref("webgl.vendor-string-override", " ");
user_pref("geo.enabled", false);
user_pref("camera.control.face_detection.enabled", false);
```

**Important:** If there is a syntax error, FF aborts parsing but settings up until that point are applied. SO check the last setting line in `user.js` from `about:config` to see if the file had any syntax errors.

**Important:** If you change the same setting from both `user.js` and `about:config`, the latter will be reset to `user.js` version after restarting FF.

You might also consider the following settings:
```
browser.safebrowsing.malware.enabled = false
browser.safebrowsing.phishing.enabled = false
media.eme.enabled = false
media.gmp-widevinecdm.enabled = false
```
