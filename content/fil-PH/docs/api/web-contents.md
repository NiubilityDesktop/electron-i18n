# webContents

> Ibigay at kontrolin ang mga web page.

Proseso: [Main](../glossary.md#main-process)

`WebContents ` ay isang [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter). Ito ay responsable sa pag-render at pagkontrol sa isang web page at bagay na ari-arian ng [`BrowserWindow`](browser-window.md). Isang halimbawa ng pag-access sa `webContents` bagay:

```javascript
const {BrowserWindow} = nangangailangan ('elektron')

hayaang manalo = bagong BrowserWindow({lapad: 800, taas: 1500})
manalo.loadURL ('http://github.com')

hayaan ang mga nilalaman =manalo.webContents
console.log (mga nilalaman)
```

## Mga Paraan

Ang mga pamamaraan na ito ay Maaaring ma-access mula sa module na ` webContents`:

```javascript
const {webContents} = require('electron')
console.log(webContents)
```

### `webContents.getAllWebContents()`

Ibinabalik `WebContents[]` - Ang array ng lahat `WebContents`ng mga pagkakataon.  . Ito ay naglalaman ng mga nilalaman ng web para sa lahat ng mga windows, webviews, binuksan devtools, at devtools karugtong ng background na mga pahina.

### `webContents.getFocusedWebContents()`

Ibinabalik `WebContents` - Ang mga nilalaman ng web na nakatuon sa application na ito, kung hindi man babalik `null`.

### `webContents.fromId(id)`

* `id` Integer

Ibinabalik ang `WebContents` - Halimbawa ng WebContents na may ibinigay na ID.

## Klase: WebContents

> Ibigay at kontrolin ang mga nilalaman na halimbawa ng BrowserWindow.

Proseso:[Main](../glossary.md#main-process)

### Halimbawa ng mga Event

#### Event: 'did-finish-load'

Binubuwag kapag ang nabigasyon ay tapos na, i.e. ang spinner ng tab ay tumigil Umiikot, at ang `onload` kaganapan ay ipinadala.

#### Event: 'did-fail-load'

Ibinabalika ang:

* `event` na Pangyayari
* `errorCode` Integer
* `errorDescription` String
* `validatedURL` String
* `isMainFrame` Boolean

Ang kaganapang ito ay tulad ng `did-finish-load` ngunit inilalabas kapag nabigo ang pag load o kinansela, hal. `window.stop() ` ay ginagamit. Ang buong listahan ng mga error code at ang kanilang mga kahulugan ay magagamit [dito](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h).

#### Event: 'did-frame-finish-load'

Ibinabalika ang:

* `event` na Pangyayari
* `ay pangunahing kuwadro` Boolean

Napalabas kapag ang frame ay nagawa na ang nabigasyon.

#### Event: 'did-start-loading'

Tumutugma sa mga puntos ng oras kapag ang spinner ng tab ay nagsimulang umikot.

#### Event: 'did-stop-loading'

Tumutugma sa mga puntos ng oras kapag ang spinner ng tab ay tumigil sa pagikot.

#### Event: 'did-get-response-details'

Ibinabalika ang:

* `event` Event
* `status` Boolean
* `newURL` String
* `originalURL` String
* `httpResponseCode` Integer
* `requestMethod` String
* ang `referer` String
* `headers` Objek
* `resourceType` String

Pinapalabas kapag may mga detalye tungkol sa hiniling na mapagkukunan at magagamit sa `katayuan` ay nagpapahiwatig sa socket connection upang i-download ang mapagkukunan.

#### Event: 'did-get-redirect-request'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `oldURL` String
* `newURL` String
* `isMainFrame` Boolean
* `httpResponseCode` Integer
* `requestMethod` String
* ang `referer` String
* `header` Bagay

Pinapalabas kapag natanggap ang pag-redirect habang humihiling ng mapagkukuhanan.

#### Event: 'dom-ready'

Ibinabalika ang:

* `event` Event

Napalabas kapag ang dokumento na ibinigay sa frame ay na-load.

#### Event: 'page-favicon-updated'

Ibinabalika ang:

* `event` Event
* `favicons` String[] - Hanay ng mga URL

Pinapalabas kapag natanggap ng pahina ang mga url ng favicon.

#### Event: 'new-window'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` String
* `frameName` String
* `disposition` String- Pwedeng `default`, `foreground-tab`, `background-tab`, `new-window`, `save-to-disk` and `other`.
* `mga pagpipilian` Object - Ang mga pagpipilian na gagamitin para sa paglikha ng bagong `BrowserWindow`.
* `Mga karagdagang tampok` String [] - Ang di-karaniwang mga tampok (mga tampok na hindi hinahawakan sa pamamagitan ng Kromo o Elektron) na ibinigay sa `window.open()`.

Lumalabas kapag ang pahina ay humiling na magbukas ng bagong bintana para sa isang `url`. Maaaring ito ay hiniling ng `window.open` o isang panlabas na link tulad ng `<a target='_blank'>`.

Sa pamamagitan ng default ng isang bagong `BrowserWindow` ay nilikha para sa `url`.

Ang pagtawag sa `kaganapan.preventDefault()` ay maiiwasan ang Elektron mula sa awtomatikong paglikha ng isang bagong `BrowserWindow`. Kung tumawag ka `event.preventDefault ()` at manu-manong pag likha ng bagong `BrowserWindow` at pagkatapos ay dapat mong itakda ang `kaganapan.newGuest` upang isangguni sa bagong `BrowserWindow` Halimbawa, ang hindi pagtupad nito ay maaaring magresulta sa hindi inaasahang asal. Halimbawa:

```javascript
myBrowserWindow.webContents.on('bagong-window', (event, url) => {
  kaganapan.preventDefault()
  const manalo = bagong BrowserWindow ({show: false})
  manalo.once ('ready-to-show', () = > win.show())
  manalo.loadURL (url)
  kaganapan.newGuest = manalo
})
```

#### Event: 'will-navigate'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` String

Ilabas kapang ang user o ang mismong page ay gustong magsimula ng nabigasyon. Ito'y pwedeng mangyari kapag ang `window.location` ng objek ay nabago o ang kiniclick ng user ang link sa page.

Ang kaganapang ito ay hindi naglalabas kapag ang navigation ay nagsimula sa programming kasama ng mga API ay tulad ng `webContents.loadURL` at `webContents.back`.

Hindi rin ito nagpapalabas para sa pag-navigate sa pahina, tulad ng pag-click sa mga link ng anchor o pag-update ng `bintana.lokasyon.hash` Gamit ang `ginawa-navigate-sa-pahina`at mga kaganapan para sa layuning ito.

Ang pagtawag sa `kaganapan.preventDefault()` ay maiiwasan ang nabigasyon.

#### Event: 'did-navigate'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` String

Nilalabas kapag natapos na ang nabigasyon.

Ang event na ito ay hindi ilalabas habang nasa nabigasyon sa loog ng page, gaya ng pag-click sa naka-ankor na mga link o naka-update ang `window.location.hash`. Gamitin ang event na `did-navigate-in-page` para sa layuning ito.

#### Event: 'did-navigate-in-page'

Ibinabalika ang:

* `kaganapan` kaganapan
* `url` Tali
* `ay pangunahing kuwadro` Boolean

Ilalabas kapag nangyari ang nabigasyon sa loob ng page.

Kapag nangyari ang nabigasyon sa loob ng page, ang URL ng page ay nababago pero hindi ito magiging dahilan sa pag-nanavigate sa labas ng page. Mga halimbawa ng mga pangyayaring ito ay kapag ang naka-ankor na mga link ay naclick o kung ang mga na DOM na `hashchange` ay natrigger.

#### Kaganapan: 'will-prevent-unload'

Ibinabalika ang:

* `kaganapan` Kaganapan

Naipalalabas kapag ang `beforeunload` ay sinusubukan ng tagahawak ng kaganapan na kanselahin ang pag-unload ng pahina.

Ang pagtawag sa `kaganapan.preventDefault()` ay hindi papansinin ang `beforeunload` tagahawak ng kaganapan at pahihintulutan ang pahina na ito ay i-unload.

```javascript
const {BrowserWindow, dialog} = nangangailangan('elektron')
const manalo = bagong BrowserWindow ({width: 800, height: 600})
manalo.webContents.on('will-prevent-unload', (kaganapan) => {
  const pagpili = dialog.showMessageBox(manalo, {
    uri: 'tanong',
    mga buton: ['Umalis', 'Manatili'],
    pamagat: 'Gusto mo bang umalis sa site na ito?',
    mensahe: 'Ang mga pagbabagong ginawa mo ay maaaring hindi mai-save.',
    defaultId: 0,
    cancelId: 1
  })
  const umalis = (pagpili === 0)
  kung (umalis) {
    kaganapan.preventDefault()
  }
})
```

#### Event: 'crashed'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `killed` Ang Boolean

Lumalabas kapag ang proseso ng tagapag-render ay nasira o pinatay.

#### Event: 'plugin-crashed'

Ibinabalika ang:

* `kaganapan` kaganapan
* `name` String
* `version` String

Lumalabas kapag ang proseso ng plugin ay nag-crash.

#### Event: 'destroyed'

Nagpapalabas kapag ang `webContents` ay nawasak.

#### Kaganapan: 'bago-input-kaganapan'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `input` Bagay - Input properties 
  * `uri` Pisi - Alinman `keyUp` o `keyDown`
  * `susi` Pisi - Katumbas ng [KeyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `code` Pisi - Katumbas ng [KeyboardEvent.code](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `isAutoRepeat` Boolean - Katumbas ng [KeyboardEvent.ulitin](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `shift` Boolean - Katumbas ng [KeyboardEvent.shiftKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `kontrol` Boolean - Katumbas ng [KeyboardEvent.controlKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `alt` Boolean - Katumbas ng [KeyboardEvent.altKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `meta` Boolean - Katumbas ng [KeyboardEvent.metaKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)

Pinapalabas bago ipadala ang `keydown` at `keyup` mga kaganapan sa pahina. Ang pagtawag sa `kaganapan.preventDefault` ay mapipigilan ang pahina `keydown`/`keyup` ng mga kaganapanat at ng shortcut sa menu.

Upang mapigilan lamang ang mga shortcut sa menu, gamitin ang [`setIgnoreMenuShortcuts`](#contentssetignoremenushortcuts):

```javascript
const {BrowserWindow} = nangangailangan('elektron')

hayaan ang panalo = bagong BrowserWindow ({lapad: 800, taas: 600})

manalo.webContents.on('bago-input-kaganapan', (kaganapan, input) => {
  // Halimbawa, paganahin lang ang mga shortcut sa keyboard ng menu ng aplikasyon kapag
  / / Ctrl/Cmd ay pababa.
  manalo.webContents.setIgnoreMenuShortcuts(!input.control && !input.meta)
})
```

#### Event: 'devtools-opened'

Ilabas kapag ang mga DevTool ay nabuksan.

#### Event: 'devtools-closed'

Ilabas kapag ang mga DevTool ay nasarado.

#### Event: 'devtools-focused'

Ilabas kapag ang mga DevTool ay napukos / nabuksan.

#### Mga event: 'certificate-error'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` Tali
* `error` String - Ang code ng error
* `certificate` [Certificate](structures/certificate.md)
* `mulingtawag` Function 
  * `isTrusted` Boolean - ay nagpapahiwatig kung ang sertipiko ay maaaring ituring na pinagkakatiwalaan

Naipalalabas kapag nabigo upang i-verify ang `sertipiko` para sa `url`.

Ang paggamit ay pareho sa [ang kaganapan `certificate-error` ng `app` ](app.md#event-certificate-error).

#### Event: 'select-client-certificate'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` Ang URL
* `certificateList` [Certificate[]](structures/certificate.md)
* `callback` Punsyon 
  * `sertipiko` [Sertipiko](structures/certificate.md) - Dapat ay isang sertipiko mula sa ibinigay na listahan

Lalabas kapag ang sertipiko ng kliyente ay hiniling.

Ang paggamit ay pareho sa [ang kaganapan `piliin-client-sertipiko`ng `app`](app.md#event-select-client-certificate).

#### Event: 'login'

Ibinabalika ang:

* `event` Ang event
* `kahilingan` Bagay 
  * `method` String
  * `url` Ang URL
  * `referrer`Ang URL
* `ang authInfo` Bagay 
  * `isProxy` Ang Boolean
  * `scheme` Ang string
  * `host` String
  * `port` Integer
  * `realm` String
* `callback` Function 
  * `username` String
  * `password` String

Lalabas kapag ang `webContents` ay gustong gawin ang basic auth.

Ang paggamit ay pareho sa [ang kaganapan `login` ng `app`](app.md#event-login).

#### Event: 'found-in-page'

Ibinabalika ang:

* `kaganapan` kaganapan
* `resulta` Bagay 
  * `requestId` Integer
  * `activeMatchOrdinal` Integer - Posisyon ng aktibong katugma.
  * `matches` Integer - Bilang ng mga Tugma.
  * `selectionArea` Objek - Mga coordinate ng unang tugmang parte.
  * `finalUpdate` Boolean

Naipalalabas kapag ang resulta ay magagamit para sa [`webContents.findInPage`] humiling.

#### Event: 'media-started-playing'

Ilabas kapag ang medya ay nagsimula.

#### Event: 'media-paused'

Ilabas kapag ang medya ay nahinto o natapos na.

#### Event: 'did-change-theme-color'

Naipalalabas kapag nagbago ang kulay ng tema ng pahina. Ito ay kadalasan dahil sa nakakaharap ng isang meta tag:

```html
<meta name='theme-color' content='#ff0000'>
```

Ibinabalika ang:

* `kaganapan` Kaganapan
* `kulay` (String | null) - Ang kulay ng tema ay nasa format na '#rrggbb'. Ito ay `null` kapag walang kulay ng tema na naka-set.

#### Event: 'update-target-url'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `url` String

Inilalabas kapag gumagalaw ang mouse sa isang link o inililipat ng keyboard ang focus sa isang link.

#### Kaganapan: 'cursor-changed'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `uri` Pisi
* `imahe` NativeImage (opsyonal)
* `scale` Lumutang (opsyonal) - scaling factor para sa mga pasadyang cursor
* `sukat` [Sukat](structures/size.md) (opsyonal) - ang sukat ng `imahe`
* `hotspot` [Punto](structures/point.md) (opsyonal) - mga coordinate ng hotspot ng pasadyang cursor

Naipalalabas kapag ang uri ng cursor ay nagbago. Ang `uri` maaring maging parameter `default`, `crosshair`, `pointer`, `text`, `maghintay`, `tulungan`, `e-resize`, `n-resize`, `ne-resize`, `nw-resize`, `s-resize`, `se-resize`, `sw-resize`, `w-resize`, `ns-resize`, `ew-resize`, `nesw-resize`, `nwse-resize`, `col-resize`, `row-resize`, `m-panning`, `e-panning`, `n-panning`, `ne-panning`, `nw-panning`, `s-panning`, `se-panning`, `sw-panning`, `w-panning`, `ilipat`, `vertical-text`, `cell`, `context-menu`, `alias`, `pagunlad`, `nodrop`, `kopya`, `none`, `hindi pwede`, `zoom-in`, `zoom-out`, `grab`, `grabbing`, `pasadya`.

Kung ang `uri` ng parameter ay `pasadya`, ang `imahe` ng parameter ang hahawak sa pinasadyang cursor ng imahe sa isang `NativeImage`, at `scale`, `laki` at `hotspot` at hawak ang karagdagang impormasyon tungkol sa mga pasadyang cursor.

#### Kaganapan: 'context-menu'

Ibinabalika ang:

* `kaganapan` kaganapan
* `params` Bagay 
  * `x` Integer - x coordinate
  * `y` Integer - y coordinate
  * `linkURL` Pisi - ang link ng URL na nakapaloob sa node sa menu ng konteksto ay tinawag sa.
  * `linkText` Pisi - Teksto na nauugnay sa link. Maaaring walang laman ang pisi kung ang mga nilalaman ng link ay isang imahe.
  * `pageURL` Pisi - ang URL ng tuktok na antas ng pahina na ang menu ng konteksto ay nananawagan.
  * `frameURL` Pisi - Ang URL ng subframe na ang menu ng konteksto ay nananawagan sa.
  * `srcURL` Pisi - pinagmulang URL para sa elemento na ang menu ng konteksto ay tinawag sa. Ang mga elemento na may mga source URL ay mga imahe, audio at video.
  * `mediaType` Pisi - Uri ng node sa menu ng konteksto ay tinawag sa. Maaaring `walang`, `imahe`, `audio`, `video`, `canvas`, `file` o `plugin`.
  * `hasImageContents` Boolean - Kung ang menu ng konteksto ay ginagamit sa isang imahe na may mga walang laman na nilalaman.
  * `isEditable` Boolean - Kung ang konteksto ay maaring i-edit.
  * `selectionText` Pisi - Teksto ng pagpili na ang menu ng konteksto ay nananawagan.
  * `titleText` Pisi - Pamagat o alt teksto ng pagpili na ang konteksto ay tinawag sa.
  * `misspelledWord` Pisi - Ang maling salita sa ilalim ng cursor, kung mayroon man.
  * `frameCharset` Pisi - Ang encoding ng karakter ng frame kung saan hiniling ang menu.
  * `inputFieldType` Pisi - Kung ang konteksto ng menu ay sinasabing sa isang patlang na input, ang uri ng patlang na iyon. Ang posibleng mga halaga ay `wala`, `plainText`, `password`, `iba pang`.
  * ` menuSourceType` Pisi - Ipasok ang pinagmulan na nagsasagawa ng konteksto ng menu. Maaaring `wala`, `mouse`, `keyboard`, `touch`, `touchMenu`.
  * `mediaFlags` Bagay - Ang mga bandila para sa elemento ng media ang menu ng konteksto ay nananawagan. 
    * `inError` Boolean - Kung ang elemento ng media ay bumagsak.
    * `isPaused` Boolean - Kung ang elemento ng media ay nakahinto.
    * `ayMuted` Boolean - Kung naka-mute ang elemento ng media.
    * `hasAudio` Boolean - Kung ang elemento ng media ay may audio.
    * `isLooping` Boolean - Kung ang elemento ng media ay looping.
    * `isControlsVisible` Boolean - Kung ang mga kontrol ng elemento ng media ay nakikita.
    * `canToggleControls` Boolean - Kung ang mga kontrol ng elemento ng media ay toggleable.
    * `canRotate` Boolean - Kung ang elemento ng media ay maaaring i-rotate.
  * `editFlags` Bagay - Ipinapahiwatig ng mga bandilang ito kung ang nanonood ay naniniwala at ito ay magagawa upang isagawa ang nararapat na pagkilos. 
    * `canUndo` Boolean - Kung naniniwala ang renderer na maaari itong ipawalang bisa.
    * `canRedo` Boolean - Kung naniniwala ang renderer na maaari itong gawing muli.
    * `canCut` Boolean - Kung naniniwala ang renderer na maaari itong i-cut.
    * `canCopy` Boolean - Kung naniniwala ang renderer na maaari itong kopyahin
    * `canPaste` Boolean - Kung naniniwala ang renderer na maaari itong i-paste.
    * `canDelete` Boolean - Kung naniniwala ang renderer na maaari itong tanggalin.
    * `canSelectAll` Boolean - Kung naniniwala ang taga-render na maaari nilang piliin ang lahat.

Lumabas kapag mayroong isang bagong menu ng konteksto na kailangang hawakan.

#### Kaganapan: 'select-bluetooth-device'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `Mga aparato` [BluetoothDevice[]](structures/bluetooth-device.md)
* `callback` Function 
  * `deviceId` na String

Ipinalalabas kapag kailangang pumili ng bluetooth device sa tawag sa `navigator.bluetooth.requestDevice`. Upang gamitin ang `navigator.bluetooth` api `webBluetooth` ay dapat na paganahin. Kung ang `kaganapan.preventDefault` ay hindi tinatawag, Ang unang magagamit na aparato ang mapipili. `callback` ay dapat na tawagin `deviceId` na mapipili, magpasa ng walang laman na pisi sa `callback` ay kanselahin ang kahilingan.

```javascript
const {app, webContents} = nangangailangan('elektron')
app.commandLine.appendSwitch ('enable-web-bluetooth')

app.on ('handa', () => {
  webContents.on('select-bluetooth-device', (kaganapan, deviceList, callback) => {
   kaganapan.preventDefault()
    hayaan ang resulta = deviceList.find((device) => {
      bumalik device.deviceName === 'test'
    })
    kung(!resulta) {
      callback('')
    } else {
      callback(resulta.deviceId)
    }
  })
})
```

#### Kaganapan: 'pintura'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `dirtyRect` [Parihaba](structures/rectangle.md)
* `imahe` [katutubong larawan](native-image.md) - Ang datos ng imahe ng buong kuwadro.

Binubuwag kapag ang bagong kuwadro ay nabuo. Tanging ang maruming lugar ay ipinasa sa buffer.

```javascript
const {BrowserWindow} = nangangailangan('elektron')

hayaan manalo ang = bagong BrowserWindow ({webPreferences: {offscreen: tama}})
manalo.webContents.on ('pintura', (kaganapan, marumi, larawan) = & gt; {
   // updateBitmap (marumi, image.kumuha ng Bitmap ())})
manalo.loadURL ('http://github.com')
```

#### Kaganapan: 'devtools-kargahan muli-ang pahina'

Binubuwag kapag ang window ng devtools ay nagtuturo sa webContents na kargahan muli

#### Kaganapan: 'naisin-isama-webview'

Ibinabalika ang:

* `kaganapan` kaganapan
* `webPreferences` Layunin - Ang mga kagustuhan sa web na gagamitin ng bisita ng pahina. Ang bagay na ito ay maaaring mabago upang ayusin ang mga kagustuhan para sa bisita ng pahina.
* `params` Layunin - Ang iba pang mga `<webview>` parameter tulad ng `src` URL. Ang bagay na ito ay maaaring baguhin upang ayusin ang mga parameter ng pahina ng bisita.

Naipalalabas kapag `<webview>` ang mga nilalaman ng web ay naka-attach sa mga nilalaman ng web na ito. Pagtawag sa `kaganapan.preventDefault()` ay sirain ang pahina ng bisita.

Maaaring gamitin ang kaganapang ito upang i-configure ang `webPreferences` para sa `webContents` ng isang `<webview>` bago ang load nito, at nagbibigay ng kakayahang magtakda upang itakda ang mga setting na hindi maaaring itakda sa pamamagitan ng `<webview>` mga katangian.

**Tandaan:** Ang tinutukoy na `preload` Ang opsyon ng script ay lilitaw bilang `preloadURL` (hindi `preload`) nasa `webPreferences` bagay na inilalabas sa kaganapang ito.

#### Kaganapan: 'ginawa-ilakip-webview'

Ibinabalika ang:

* `kaganapan` Kaganapan
* `WebContents` WebContents - Ang mga nilalaman ng bisita ng web na ginagamit ang `<webview>`.

Naipalalabas kapag ang isang `<webview>` ay naka-attach sa mga nilalaman ng web na ito.

#### Kaganapan'console-mensahe'

Ibinabalik ang:

* `level` Integer
* `mensahe` Pisi
* `linya` Integer
* `sourceId` Pisi

Pinapalabas kapag nag-log ang nauugnay na window sa isang mensahe ng console. Hindi ipapalabas para sa mga window na may *offscreen rendering* na pinapagana.

### Instance Methods

#### `contents.loadURL(url[, mga pagpipilian])`

* `url` String
* `options` Mga bagay (opsyonal) 
  * `httpReferrer` Pisi (opsyonal) - Isang HTTP Referrer url.
  * `userAgent` Pisi (opsyonal) - Isang ahenteg gumagamit na nagmumula sa kahilingan.
  * `extraHeaders` Pisi (opsyonal) - Mga dagdag na header na pinaghihiwalay ng "\n"
  * `postData` ([UploadRawData[]](structures/upload-raw-data.md) | [UploadFile[]](structures/upload-file.md) | [UploadFileSystem[]](structures/upload-file-system.md) | [UploadBlob[]](structures/upload-blob.md)) - (opsyonal)
  * `baseURLForDataURL` String(opsyonal) - Basi nag url (may tagapahiwalay sa landas ng separator) para sa mga dokumento na kakargahin sa pamamagitan ng datos ng url. Ito ay kailangan kung ang tinutukoy ng `url` iy isang datos ng url at kailangan maikarga sa ibang dokumento.

Naglo-load ang `url` sa bintana. Ang `url` ay dapat naglalaman ng prefix ng protocol, hal. ang `http://` o `file://`. Kung ang load ay dapat mag-bypass http cache pagkatapos gamitin ang `pragma` header upang makamit ito.

```javascript
const {webContents} = nangangailangan('elektron')
const mga pagpiilian = {extraHeaders: 'pragma: no-cache\n'}
webContents.loadURL('https://github.com', mga pagpipilian)
```

#### `mga nilalaman.downloadURL(url)`

* `url` Tali

Nagsimula ang pag-download ng mapagkukunan sa `url` nang walang pag-navigate. Ang `will-download` kaganapan ng `session` ay ma-trigger.

#### `mga nilalaman.getURL()`

Ibinabalik `Pisi` - Ang URL ng kasalukuyang web page.

```javascript
const {BrowserWindow} = nangangailangan('elektron')
hayaan ang panalo = bagong BrowserWindow ({lapad: 800, taas: 600})
manalo.loadURL ('http://github.com')

hayaan ang kasalukuyangURL = manalo.webContents.getURL()
console.log(currentURL)
```

#### `mga nilalaman.getTitle()`

Ibinabalik `Pisi` - Ang pamagat ng kasalukuyang web page.

#### `mga nilalaman.isDestroyed()`

Ibinabalik `Boolean` - Kung ang web page ay nawasak.

#### `mga nilalaman.focus()`

Nakatutok sa web page.

#### `mga nilalaman.isFocused()`

Ibinabalik `Boolean` - Kung nakatutok ang web page.

#### `mga nilalaman.isLoading()`

Ibinabalik `Boolean` - Kung ang pahina ng web ay naglo-load ng mga mapagkukunan.

#### `mga nilalaman.isLoadingMainFrame()`

Ibinabalik `Boolean` - Kung ang pangunahing frame (at hindi lamang mga iframe o mga frame sa loob nito) ay naglo-load pa rin.

#### `mga nilalaman.isWaitingForResponse()`

Ibinabalik `Boolean` - Kung naghihintay ang pahina ng web para sa unang tugon mula sa pangunahing mapagkukunan ng pahina.

#### `mga nilalaman.ihinto()`

Nagtitigil ng anumang nakabingbing na nabigasyon.

#### `mga nilalaman.reload()`

I-reload ang kasalukuyang web page.

#### `mga nilalaman.reloadIgnoringCache()`

Mga Reloads ng kasalukuyang pahina at binabalewala ang cache.

#### `mga nilalaman.canGoBack()`

Ibinabalik `Boolean` - Kung ang browser ay maaring bumalik sa nakaraang pahina ng web.

#### `mga nilalaman.canGoForward()`

Ibinabalik `Boolean` - Kung ang browser ay maaring magpatuloy sa susunod na pahina ng web.

#### `mga nilalaman.canGoToOffset(offset)`

* `offset` Integer

Ibinabalik `Boolean` - Kung ang pahina ng web ay maaring pumunta sa `offset`.

#### `mga nilalaman.clearHistory()`

Nililimas ang kasaysayan ng pag-navigate.

#### `mga nilalaman.goBack()`

Ginagawang bumalik ang browser sa isang pahina ng web.

#### `mga nilalaman.goForward()`

Ginagawa ang browser na pumunta sa isang pahina ng web.

#### `mga nilalaman.goToIndex(index)`

* `index` Integer

Naka-navigate ang browser sa tinukoy na ganap sa pahina ng web na index.

#### `mga nilalaman.goToOffset(offset)`

* `offset` Integer

Ang nabigasyon sa tinutukoy na offset galing sa "kasalukuyang entri".

#### `mga nilalaman.isCrashed()`

Ibinabalik `Boolean` - Kapag ang proseso ng tagapag-render ay nawasak.

#### `mga nilalaman.setUserAgent(userAgent)`

* `userAgent` String

Naka-override ang ahenteng gumagamit para sa pahina ng web na ito.

#### `mga nilalaman.getUserAgent()`

Ibinabalik`Pisi` - Ang ahenteng gumagamit para sa pahina ng web na ito.

#### `mga nilalaman.insertCSS(css)`

* `css` Pisi

Mga pagturok ng CSS sa kasalukuyang pahina ng web.

#### `mga nilalaman.executeJavaScript(code[, userGesture, mulingtumawag])`

* `code` String
* `userGesture` Boolean (opsyonal) - Default ay `huwad`.
* `callback` Function (opsyonal) - Tinawagan pagkatapos na maisakatuparan ang iskrip. 
  * `result` Any

Ibinabalik ang mga `Pangako` - Ang isang pangako na lumulutas sa resulta ng naipatupad na code o tinanggihan kung ang resulta ng code ay isang tinanggihang pangako.

Sinusuri ang mga `code` sa pahina.

Sa window ng browser ang ilang mga HTML API tulad ng `requestFullScreen` ay maaari lamang nananawagan ng kilos mula sa gumagamit. Ang pagtatakda ng `userGesture` sa `totoo` ay alisin ang limitasyon na ito.

Kung ang resulta ng code na isinagawa ay isang pangako ang resulta ng muling pagtawag ay nalutas na halaga ng pangako. Inirerekomenda naming gamitin mo ang ibinalik na Pangako upang mahawakan ang code na nagreresulta sa isang Pangako.

```js
contents.executeJavaScript ('fetch("https://jsonplaceholder.typicode.com/users/1").
pagkatapos(resp => resp.json())', totoo)
  . pagkatapos((resulta) => {
    console.log(resulta) // Ang magiging JSON na bagay mula sa pagkuha ng tawag
  })
```

#### `contents.setIgnoreMenuShortcuts(huwag pansinin)` *Eksperimento*

* `huwag pansinin` Boolean

Huwag pansinin ang mga shorcut menu ng aplikasyon habang ang mga nilalaman ng web na ito ay nakatuon.

#### `contents.setAudioMuted(naka-mute)`

* `muted` Boolean

I-mute ang audio sa kasalukuyang web na page.

#### `mga nilalaman.ng AudioMuted()`

Bumalik `Boolean` - Kung naka-mute ang pahinang ito.

#### `mga nilalaman.setZoomFactor(kadahilanan)`

* `kadahilanan`Numero - Zoom factor.

Binabago ang factor ng pag-zoom sa tinukoy na factor. Ang factor ng pag-zoom ay porsiyento ng zoom na hinati sa 100, so 300% = 3.0.

#### `mga nilalaman.getZoomFactor(callback)`

* `callback` Punsyon 
  * `zoomFactor` Numero

Nagpapadala ng kahilingan upang makakuha ng kasalukuyang factor ng pag-zoom, `callback `ay tinatawag na `callback(zoomFactor)`.

#### `mga nilalaman.setZoomLevel(antas)`

* `antas` Numero - antas ng Zoom

Binabago ang antas ng pag-zoom para sa tinitiyak na antas. Ang orihinal na laki ng 0 at bawat isa Ang pagdagdag sa pagtaas o sa pagbaba ay kumakatawan sa pag-zooming ng 20% na mas malaki o mas maliit sa default mga limitasyon ng 300% at 50% ng orihinal na laki, ayon sa pagkakabanggit.

#### `mga nilalaman.getZoomLevel (callback)`

* `callback` Punsyon 
  * `zoomLevel` Numero

Tinatapos ang isang kahilingan upang makakuha ng kasalukuyang antas sa pag-zoom,`callback`ay tinatawag na `callback(zoomLevel)`.

#### `mga nilalaman.setZoomLevelLimits (pinakamaliit na Antas, pinakamataas na Antas)`

* `pinakamaliitna Antas` na Numero
* `Pinakamataas na Antas` na Numero

**Deprecated:** Tumawag `setVisualZoomLevelLimits` sa halip na itakda ang visual zoom ng mga limitasyon sa antas. Ang paraan na ito ay aalisin sa Elektron 2.0.

#### `mga nilalaman.setVisualZoomLevelLimits(pinakamababang antas, pinakamataas na antas)`

* `pinakamaliitna Antas` na Numero
* `Pinakamataas na Antas` na Numero

Itinatakda ang pinakamataas at pinakamababang antas ng pinch-sa-zoom.

#### `mga nilalaman.setVisualZoomLevelLimits (pinakamababang antas, pinakamataas na antas)`

* `pinakamaliitna Antas` na Numero
* `Pinakamataas na Antas` na Numero

Nagtatakda ng pinakamataas at pinakamababa na antas batay sa layout (i.e hindi visual) na antas ng zoom.

#### `mga nilalaman.undo()`

Pinapatupad ang command sa pag-edit `undo` sa pahina ng web.

#### `mga nilalaman.redo()`

Pinapatupad ang command sa pag-edit `gawing muli` sa pahina ng web.

#### `mga nilalaman.cut()`

Pinapatupad ang utos sa pag-edit `hiwa` sa pahina ng web.

#### `mga nilalaman.copy()`

Ang pagpapatupad ng utos sa pag-edit `kopya` sa pahina ng web.

#### `mga nilalaman.copyImageAt(x, y)`

* `x` Integer
* `y` Integer

Kopyahin ang larawan sa ibinigay na posisyon sa clipboard.

#### `mga nilalaman.paste()`

Pinapatupad ang utos sa pag-edit `paste` sa pahina ng web.

#### `mga nilalaman.pasteAndMatchStyle()`

Ang pagpapatupad ng utos sa pag-edit `pasteAndMatchStyle` sa pahina ng web.

#### `mga nilalaman.delete()`

Pinapatupad ang utos sa pag-edit `tanggalin` sa pahina ng web.

#### `mga nilalaman.selectAll()`

Pinapatupad ang utos sa pag-edit `selectAll` sa pahina ng web.

#### `mga nilalaman.unselect()`

Pinapatupad ang utos sa pag-edit `unselect` sa pahina ng web.

#### `mga nilalaman.replace(text)`

* `text` String

Pinapatupad ang utos sa pag-edit ` palitan ` sa web page.

#### `mga nilalaman. ibalik ang maling pagbaybay(teksto)`

* `text` String

Pinapatupad ang utos sa pag-edit ` palitan ang maling pagbaybay ` sa web page.

#### `mga nilalaman.ipasok ang teksto(teksto)`

* `text` String

Pagsingit `text` para sa nakapukos na elemento.

#### `mga nilalaman.findInPage (teksto [, mga pagpipilian])`

* `teksto` String - Ang nilalaman na hahanapin, ay hindi dapat walang laman.
* `options` Bagay (opsyonal) 
  * `abanti` Boolean - (opsyonal) Kung mananaliksik ka ng paabanti o patalikod, defaults sa `true`.
  * `findNext` Boolean - (opsyonal) Kung ang operasyon isang kahilingan o isang pagsasagawang kasunod, mga defaults sa `false`.
  * `matchCase` Boolean - (opsyonal) Kung saan ang paghahanap ay dapat case-sensitive, mga defaults sa `false`.
  * `wordStart` Boolean - (opsyonal) Kung saan maghahanap ka lang ng simula ng salita. mga defaults sa `false`.
  * `medialCapitalAsWordStart` Boolean - (opsyonal) Kung ang pinagsama na may `wordStart`, tinatanggap ang kapareha sa gitna ng salita at kung ang kapareha nag nagsimula ng malaking titik at sinundan ng maliit na titik o walang-letter. Tinatanggap ang ilan na ibang intra-salitang magkapareha, mga defaults `false`.

Ibinabalik `Integer` - Ang kahilingang id na ginagamit para sa kahilingan.

Magsisimula ng isang kahilingan upang mahanap ang lahat ng mga tugma para sa `text` sa pahina ng web. Ang resulta ng kahilingan ay maaaring makuha sa pamamagitan ng pag-subscribe sa [`found-in-page`](web-contents.md#event-found-in-page) kaganapan.

#### `mga nilalaman.stopFindInPage(aksyon)`

* `aksyon` String - Tinutukoy ang aksyon upang maganap kapag nagtatapos [`webContents.findInPage`] kahilingan. 
  * `clearSelection` - Tanggalin ang mga napili.
  * `keepSelection` - Isalin ang seleksyon sa isang normal na seleksyon.
  * `activateSelect` - Tumuon at i-click ang node ng pagpili.

Hinihinto ang `findInPage` kahilingan para sa `webContents` kasama ang ibinigay na `aksyon`.

```javascript
const {webContents} = nangangailangan('elektron')
webContents.on('found-in-page', (kaganapan, resulta) => {
  kung (resulta.finalUpdate) webContents.stopFindInPage('clearSelection')
})

const requestId = webContents.findInPage('api')
console.log(requestId)
```

#### `mga nilalaman.capturePage([rect, ]callback)`

* `rect` [Rectangle](structures/rectangle.md) (opsyonal) - Ang kabuuan ng page na kukuhanin
* `tumawag muli` Punsyon 
  * `imahe` [NativeImage](native-image.md)

Kumukuha ng isang snapshot ng pahina sa loob ng `rect`. Sa oras na makumpleto `callback` ay tatawagan `callback(imahe)`. Ang `imahe` ay isang halimbawa ng [NativeImage](native-image.md) na nag-iimbak ng data ng snapshot. Umupo `rect` ay kukunin ang buong nakikitang pahina.

#### `contents.hasServiceWorker(callback)`

* `callback` Ang Punsyon 
  * `hasWorker` Boolean

Sinusuri kung ang anumang ServiceWorker ay nakarehistro at nagbabalik ng isang boolean bilang tugon sa `callback`.

#### `contents.unregisterServiceWorker(callback)`

* `callback` Ang Punsyon 
  * `tagumpay` Boolean

Unregister ang anumang ServiceWorker kung kasalukuyan at nagbabalik ng isang boolean bilang tugon sa `callback` kapag ang pangako ng JS ay natupad o hindi totoo kapag ang pangako ng JS ay tinanggihan.

#### `mga nilalaman.getPrinters()`

Kunin ang listahan ng sistema ng printer.

Ibinabalik [`PrinterInfo[]`](structures/printer-info.md)

#### `mga nilalaman.print([options], [callback])`

* `mga opsyon` Bagay (opsyonal) 
  * `silent` Boolean (opsyonal) - Huwag itanong sa user sa mga setting sa pagpapaimprinta. Ang naka-default ay `false`.
  * `printBackground` Boolean (opsyonal) - Iniimprinta rin ang kulay ng background at ang mukha ng web page. Ang naka-default ay `false`.
  * `deviceName` String (opsyonal) - Itakda ang pangalan ng gagamiting printer na gagamitin. Ang naka-default ay `"`.
* `callback` Function (opsyonal) 
  * tagumpay` Boolean - Nagpapahiwatig ng tagumpay ng naka-print na tawag.

Nagpiprint ng pahina ng web sa mga window. Kapag ang `tahimik` ay naka-set sa `totoo`, ang Elektron ay pipiliin ang sistema ng default na printer kung ang `deviceName` ay walang laman at ang mga default na magtatakda para sa pag-print.

Pagtawag sa `window.print()` sa pahina ng web ay katumbas ng pagtawag sa `webContents.print({silent: false, printBackground: false, deviceName: ''})`.

Gamitin ang `pahina-pahinga-bago: laging;` istilo ng CSS upang pilitin na i-print sa isang bagong pahina.

#### `contents.printToPDF(options, callback)`

* `mga opsyon` Bagay 
  * `marginsType` Integer - (opsyonal) Itinatakda ang uri ng mga margin na gagamitin. Gumagamit ng 0 para sa naka-default na margin, 1 para sa walang margin, at 2 para sa pinakamaliit na maaaring gawing margin.
  * `pageSize` String - (opsyonal) Itinatakda ang sukat ng page ng nalilikhang PDF. Pwedeng `A3`, `A4`, `A5`, `Legal`, `Letter`, `Tabloid` o ang Objek na mayroong `height` at `width` na naka-micron.
  * `printBackground` Boolean - (opsyonal) Pwedeng i-imprinta ang mga background ng CSS.
  * `printSelectionOnly` Boolean - (opsyonal) Pwedeng i-imprinta ang mga napili lamang.
  * `landscape` Boolean - (opsyonal) `true` para sa landscape, `false` para sa portrait.
* `callback` Ang Punsyon 
  * `error` Error
  * `data` Buffer

Ang pagpiprint ng pahina ng web ng window bilang PDF kasama ng kromo sa pag preview ng imprenta ng pasadyang mga nagtatakda.

Ang `callback` ay tatawagan na may `callback(error, data)` sa pagkumpleto. Ang `data` ay isang `Buffer` na naglalaman ng nabuong datos ng PDF.

Ang `tanawin` hindi papansinin kung ang `@pahina` CSS sa-panuntunan ay ginagamit sa pahina ng web.

Bilang default, ang isang walang laman na `mga pagpipilian` ay itinuturing na:

```javascript
{
  marginsType: 0,
  printBackground: huwad,
  printSelectionOnly: huwad,
  tanawin: huwad
}
```

Gamitin ang `pahina-pahinga-bago: laging;` istilo ng CSS upang pilitin na i-print sa isang bagong pahina.

An example of `webContents.printToPDF`:

```javascript
const {BrowserWindow} = require('electron')
const fs = require('fs')

let win = new BrowserWindow({width: 800, height: 600})
win.loadURL('http://github.com')

win.webContents.on('did-finish-load', () => {
  // Use default printing options
  win.webContents.printToPDF({}, (error, data) => {
    if (error) throw error
    fs.writeFile('/tmp/print.pdf', data, (error) => {
      if (error) throw error
      console.log('Write PDF successfully.')
    })
  })
})
```

#### `contents.addWorkSpace(path)`

* `path` String

Adds the specified path to DevTools workspace. Must be used after DevTools creation:

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()
win.webContents.on('devtools-opened', () => {
  win.webContents.addWorkSpace(__dirname)
})
```

#### `contents.removeWorkSpace(path)`

* `path` String

Removes the specified path from DevTools workspace.

#### `contents.openDevTools([options])`

* `options` Bagay (opsyonal) 
  * `mode` String - Opens the devtools with specified dock state, can be `right`, `bottom`, `undocked`, `detach`. Defaults to last used dock state. In `undocked` mode it's possible to dock back. In `detach` mode it's not.

Opens the devtools.

#### `contents.closeDevTools()`

Closes the devtools.

#### `contents.isDevToolsOpened()`

Returns `Boolean` - Whether the devtools is opened.

#### `contents.isDevToolsFocused()`

Returns `Boolean` - Whether the devtools view is focused .

#### `contents.toggleDevTools()`

Toggles the developer tools.

#### `contents.inspectElement(x, y)`

* `x` Integer
* `y` Integer

Starts inspecting element at position (`x`, `y`).

#### `contents.inspectServiceWorker()`

Opens the developer tools for the service worker context.

#### `contents.send(channel[, arg1][, arg2][, ...])`

* `channel` String
* `...args` anuman[]

Magpadala ng mensahe na asynchronous para maisagawa ang proseso sa pamamagitan ng `channel`. pwede mo ring ipadala ang mga argumento na arbitraryo. Ang mga argumento ay maaaring ilalathala ng baha-bahagi sa loob ng JSON at dahil dito walang mga punsyon o ugnay-ugnay na modelo ang maaaring isama.

The renderer process can handle the message by listening to `channel` with the `ipcRenderer` module.

An example of sending messages from the main process to the renderer process:

```javascript
// Sa mga pangunahing proseso.
const {app, BrowserWindow} = require('electron')
let win = null

app.on('ready', () => {
  win = new BrowserWindow({width: 800, height: 600})
  win.loadURL(`file://${__dirname}/index.html`)
  win.webContents.on('did-finish-load', () => {
    win.webContents.send('ping', 'whoooooooh!')
  })
})
```

```html
<!-- index.html -->
<html>
<body>
  <script>
    require('electron').ipcRenderer.on('ping', (event, message) => {
      console.log(message)  // Prints 'whoooooooh!'
    })
  </script>
</body>
</html>
```

#### `contents.enableDeviceEmulation(parameters)`

* `parameters` Bagay 
  * `screenPosition` String - Specify the screen type to emulate (default: `desktop`) 
    * `desktop` - Desktop screen type
    * `mobile` - Mobile screen type
  * `screenSize` [Size](structures/size.md) - Set the emulated screen size (screenPosition == mobile)
  * `viewPosition` [Point](structures/point.md) - Position the view on the screen (screenPosition == mobile) (default: `{x: 0, y: 0}`)
  * `deviceScaleFactor` Integer - Set the device scale factor (if zero defaults to original device scale factor) (default: ``)
  * `viewSize` [Size](structures/size.md) - Set the emulated view size (empty means no override)
  * `fitToView` Boolean - Whether emulated view should be scaled down if necessary to fit into available space (default: `false`)
  * `offset` [Point](structures/point.md) - Offset of the emulated view inside available space (not in fit to view mode) (default: `{x: 0, y: 0}`)
  * `scale` Float - Scale of emulated view inside available space (not in fit to view mode) (default: `1`)

Enable device emulation with the given parameters.

#### `contents.disableDeviceEmulation()`

Disable device emulation enabled by `webContents.enableDeviceEmulation`.

#### `contents.sendInputEvent(event)`

* `event` Bagay 
  * `type` String (**required**) - The type of the event, can be `mouseDown`, `mouseUp`, `mouseEnter`, `mouseLeave`, `contextMenu`, `mouseWheel`, `mouseMove`, `keyDown`, `keyUp`, `char`.
  * `modifiers` String[] - An array of modifiers of the event, can include `shift`, `control`, `alt`, `meta`, `isKeypad`, `isAutoRepeat`, `leftButtonDown`, `middleButtonDown`, `rightButtonDown`, `capsLock`, `numLock`, `left`, `right`.

Sends an input `event` to the page. **Note:** The `BrowserWindow` containing the contents needs to be focused for `sendInputEvent()` to work.

For keyboard events, the `event` object also have following properties:

* `keyCode` String (**required**) - The character that will be sent as the keyboard event. Should only use the valid key codes in [Accelerator](accelerator.md).

For mouse events, the `event` object also have following properties:

* `x` Integer (**required**)
* `y` Integer (**required**)
* `button` String - The button pressed, can be `left`, `middle`, `right`
* `globalX` Integer
* `globalY` Integer
* `movementX` Integer
* `movementY` Integer
* `clickCount` Integer

For the `mouseWheel` event, the `event` object also have following properties:

* `deltaX` Integer
* `deltaY` Integer
* `wheelTicksX` Integer
* `wheelTicksY` Integer
* `accelerationRatioX` Integer
* `accelerationRatioY` Integer
* `hasPreciseScrollingDeltas` Boolean
* `canScroll` Boolean

#### `contents.beginFrameSubscription([onlyDirty ,]callback)`

* `onlyDirty` Boolean (optional) - Defaults to `false`
* `callback` Function 
  * `frameBuffer` Buffer
  * `dirtyRect` [Parihaba](structures/rectangle.md)

Begin subscribing for presentation events and captured frames, the `callback` will be called with `callback(frameBuffer, dirtyRect)` when there is a presentation event.

The `frameBuffer` is a `Buffer` that contains raw pixel data. On most machines, the pixel data is effectively stored in 32bit BGRA format, but the actual representation depends on the endianness of the processor (most modern processors are little-endian, on machines with big-endian processors the data is in 32bit ARGB format).

The `dirtyRect` is an object with `x, y, width, height` properties that describes which part of the page was repainted. If `onlyDirty` is set to `true`, `frameBuffer` will only contain the repainted area. `onlyDirty` defaults to `false`.

#### `contents.endFrameSubscription()`

End subscribing for frame presentation events.

#### `contents.startDrag(item)`

* `item` Bagay 
  * `file` String or `files` Array - The path(s) to the file(s) being dragged.
  * `icon` [NativeImage](native-image.md) - The image must be non-empty on macOS.

Sets the `item` as dragging item for current drag-drop operation, `file` is the absolute path of the file to be dragged, and `icon` is the image showing under the cursor when dragging.

#### `contents.savePage(fullPath, saveType, callback)`

* `fullPath` String - The full file path.
* `saveType` String - Specify the save type. 
  * `HTMLOnly` - Save only the HTML of the page.
  * `HTMLComplete` - Save complete-html page.
  * `MHTML` - Save complete-html page as MHTML.
* `callback` Function - `(error) => {}`. 
  * `error` Error

Returns `Boolean` - true if the process of saving page has been initiated successfully.

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()

win.loadURL('https://github.com')

win.webContents.on('did-finish-load', () => {
  win.webContents.savePage('/tmp/test.html', 'HTMLComplete', (error) => {
    if (!error) console.log('Save page successfully')
  })
})
```

#### `contents.showDefinitionForSelection()` *macOS*

Pinapakita ang pop-up na diksyonaryo na naghahanap ng mga napiling salita sa page.

#### `contents.setSize(options)`

Set the size of the page. This is only supported for `<webview>` guest contents.

* `options` Bagay 
  * `normal` Object (optional) - Normal size of the page. This can be used in combination with the [`disableguestresize`](web-view-tag.md#disableguestresize) attribute to manually resize the webview guest contents. 
    * `width` Integer
    * `height` Integer

#### `contents.isOffscreen()`

Returns `Boolean` - Indicates whether *offscreen rendering* is enabled.

#### `contents.startPainting()`

If *offscreen rendering* is enabled and not painting, start painting.

#### `contents.stopPainting()`

If *offscreen rendering* is enabled and painting, stop painting.

#### `contents.isPainting()`

Returns `Boolean` - If *offscreen rendering* is enabled returns whether it is currently painting.

#### `contents.setFrameRate(fps)`

* `fps` Integer

If *offscreen rendering* is enabled sets the frame rate to the specified number. Only values between 1 and 60 are accepted.

#### `contents.getFrameRate()`

Returns `Integer` - If *offscreen rendering* is enabled returns the current frame rate.

#### `contents.invalidate()`

Schedules a full repaint of the window this web contents is in.

If *offscreen rendering* is enabled invalidates the frame and generates a new one through the `'paint'` event.

#### `contents.getWebRTCIPHandlingPolicy()`

Returns `String` - Returns the WebRTC IP Handling Policy.

#### `contents.setWebRTCIPHandlingPolicy(policy)`

* `policy` String - Specify the WebRTC IP Handling Policy. 
  * `default` - Exposes user's public and local IPs. This is the default behavior. When this policy is used, WebRTC has the right to enumerate all interfaces and bind them to discover public interfaces.
  * `default_public_interface_only` - Exposes user's public IP, but does not expose user's local IP. When this policy is used, WebRTC should only use the default route used by http. This doesn't expose any local addresses.
  * `default_public_and_private_interfaces` - Exposes user's public and local IPs. When this policy is used, WebRTC should only use the default route used by http. This also exposes the associated default private address. Default route is the route chosen by the OS on a multi-homed endpoint.
  * `disable_non_proxied_udp` - Does not expose public or local IPs. When this policy is used, WebRTC should only use TCP to contact peers or servers unless the proxy server supports UDP.

Setting the WebRTC IP handling policy allows you to control which IPs are exposed via WebRTC. See [BrowserLeaks](https://browserleaks.com/webrtc) for more details.

#### `contents.getOSProcessId()`

Returns `Integer` - The `pid` of the associated renderer process.

### Mga Katangian ng Instance

#### `contents.id`

A `Integer` representing the unique ID of this WebContents.

#### `contents.session`

A [`Session`](session.md) used by this webContents.

#### `contents.hostWebContents`

A [`WebContents`](web-contents.md) instance that might own this `WebContents`.

#### `contents.devToolsWebContents`

A `WebContents` of DevTools for this `WebContents`.

**Note:** Users should never store this object because it may become `null` when the DevTools has been closed.

#### `contents.debugger`

A [Debugger](debugger.md) instance for this webContents.