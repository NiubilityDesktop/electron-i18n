# Upgrading Chrome Checklist

Dieses Dokument ist eine Übersicht der nötigen Schritte bei jedem Chrome Upgrade in Electron.

Die folgende Auflistung beinhaltet zusätzliche Schritte für das updaten des Electron Codes für jede Chrom/Node API Änderung.

- Sicherstellen, dass die neue Chrome Version hier verfügbar ist: https://github.com/zcbenz/chromium-source-tarball/releases
- Update die `VERSION`-Datei im Root-Verzeichnis des `electron/libchromiumcontent` Repository
- Update the `CLANG_REVISION` in `script/update-clang.sh` to match the version Chrome is using in `libchromiumcontent/src/tools/clang/scripts/update.py`
- Upgrade `vendor/node` to the Node release that corresponds to the v8 version being used in the new Chrome release. See the v8 versions in Node on https://nodejs.org/en/download/releases for more details
- Upgrade `vendor/crashpad` for any crash reporter changes needed
- Upgrade `vendor/depot_tools` for any build tools changes needed
- Update the `libchromiumcontent` SHA-1 to download in `script/lib/config.py`
- Open a pull request on `electron/libchromiumcontent` with the changes
- Open a pull request on `electron/electron` with the changes 
  - This should include upgrading the submodules in `vendor/` as needed
- Verify debug builds succeed on: 
  - macOS
  - 32-bit Windows
  - 64-bit Window
  - 32-bit Linux
  - 64-bit Linux
  - ARM Linux
- Verify release builds succeed on: 
  - macOS
  - 32-bit Windows
  - 64-bit Window
  - 32-bit Linux
  - 64-bit Linux
  - ARM Linux
- Verify tests pass on: 
  - macOS
  - 32-bit Windows
  - 64-bit Window
  - 32-bit Linux
  - 64-bit Linux
  - ARM Linux

## Verify ffmpeg Support

Electron ships with a version of `ffmpeg` that includes proprietary codecs by default. A version without these codecs is built and distributed with each release as well. Each Chrome upgrade should verify that switching this version is still supported.

You can verify Electron's support for multiple `ffmpeg` builds by loading the following page. It should work with the default `ffmpeg` library distributed with Electron and not work with the `ffmpeg` library built without proprietary codecs.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Proprietary Codec Check</title>
  </head>
  <body>
    <p>Checking if Electron is using proprietary codecs by loading video from http://www.quirksmode.org/html5/videos/big_buck_bunny.mp4</p>
    <p id="outcome"></p>
    <video style="display:none" src="http://www.quirksmode.org/html5/videos/big_buck_bunny.mp4" autoplay></video>
    <script>
      const video = document.querySelector('video')
      video.addEventListener('error', ({target}) => {
        if (target.error.code === target.error.MEDIA_ERR_SRC_NOT_SUPPORTED) {
          document.querySelector('#outcome').textContent = 'Not using proprietary codecs, video emitted source not supported error event.'
        } else {
          document.querySelector('#outcome').textContent = `Unexpected error: ${target.error.code}`
        }
      })
      video.addEventListener('playing', () => {
        document.querySelector('#outcome').textContent = 'Using proprietary codecs, video started playing.'
      })
    </script>
  </body>
</html>
```

## Links

- [Chrome Release Schedule](https://www.chromium.org/developers/calendar)