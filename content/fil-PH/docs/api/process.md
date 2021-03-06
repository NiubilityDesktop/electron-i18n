# proseso

> Karugtong sa prosesong bagay.

Proseso:[Pangunahin](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

Ang `prosesong` bagay ng Electron ay pinalawak mula sa [Node.js `proseso` bagay](https://nodejs.org/api/process.html). Ito ay nagdaragdag ng mga sumusunod na pangyayari, katangian, at mga pamamaraan:

## Pangyayari

### Pangyayari: 'puno'

Napalabas kapag na-load ng Electron ang kanyang panloob na inisyalisasyon iskrip at simulang mag load ang web page o sa pangunahin iskrip.

Pwede rin itong magamit ng preload iskrip para magdagdag ang tinanggal na Node global na simbolo pabalik sa global scope kung ang integrasyon ng node ay nakapatay. 

```javascript
// preload.js
const _setImmediate = setImmediate
const _clearImmediate = clearImmediate
process.once('loaded', () => {
  global.setImmediate = _setImmediate
  global.clearImmediate = _clearImmediate
})
```

## Mga Katangian

### `proseso.defaultApp`

Ang `Boolean`. Kung ang app ay nagsimula sa pamamagitan ng ipinapasa bilang parametro sa default app, ang katangiang ito ay `totoo` sa pangunahing proseso, kunghindiman ito ay `malabo`

### `proseso.mas`

Ang `Boolean`. Para sa itinayo na Mac App Store, ang propyedad na ito ay `totoo`, para sa ibang initayo ito ay `malabo`.

### `proseso.noAsar`

Ang `Boolean` na nag kontrol ng ASAR ay nagsuporta sa loob ng iyong aplikasyon. Ang pag set nito sa `totoo` ay hindi mapapagana ang suporta para `asar`arkibos sa Node's built-in modyul.

### `proseso.noDeprecation`

Ang `Boolean` na nag kokontrol kung ang mga babala ng deprecation ay ililimbag sa `stderr`.  
Patatakda nito sa `totoo` ay patatahimikin ang babala ng deprecation. Ang propeyedad na ito ai ginagamit sa halip na `--walang-deprecation` nagt-utos ng linya ng bandila.

### `proseso.pinagkukunanPath`

Ang `String` nag representa ng landas patungo sa pangunahing panuto.

### `proseso.itaponDeprecation`

Ang `Boolean` na kumokontrol kung o hindi ang mga babala sa deprecation ay matatapon bilang eskepsyon. Ang pagtatakda ng mga ito na `totoo` ay magtatapon ng mali para sa deprecations. Ang propeyedad na ito ay ginagamit sa halip na `--tapon-deprecation` naguutos sa bandilang linya.

### `proseso.bakasDeprecation`

Ang `Boolean` na nagkontrol kung o hindi ang deprecation ay nakalimbag sa `stderr` isinama ng isinalansan na bakas. Ang pagtatakda nito sa `totoo` ay maglilimbag ng isinalansan na bakas. Ang propeyedad na ito ay sa halip na ang `--bakas-deprecation` naguutos ng linyang bandila.

### `proseso.bakasProsesoBabala`

Ang `Boolean` na nagkontrol kung o hindi na ang mga babalang proseso ay nakalimbag sa `stderr` isama sa isinalansan na bakas. Ang pagtatakda nito sa `totoo` ay maglilimbag nga isinalansan na bakas para sa mga babalang proseso (kasama ang deprecation). Ang propeyedad na ito ay sa halip sa `--bakas-babala`nag uutos na linya ng bandila.

### `proseso.uri`

Ang `String` ay nagrepresenta sa kasalukuyang prosesong uri, pwede ring `"browser"`(i.e pangunahing proses) o `"gumawa"`.

### `proseso.bersyon.chrome`

Ang `String` nagrepresenta sa bersyon ng Chrome string.

### `proseso.bersyon.electron`

Ang `String` nag representang bersyon ng Electron string.

### `proseso.windowsStore`

Ang `Boolean`. Kung ang app ay tumatakbo bilang Windows Store app (appx), ang propeyedad a `totoo`, para kung hindimna ito ay `malabo`.

## Pamamaraan

Ang `proseso` na bagay ay may mga sumusunod na paraan:

### `proseso.crash()`

Ang mga dahilan ng pangunahing thread sa kasalukuyang proseso ay lumagpak.

### `proseso.getCPUUsage()`

Pagbabalik [` CPUUsage `](structures/cpu-usage.md)

### ` proseso.kuhaIOCounter()`*Windows**Linux*

Pagbabalik [`IOCounters`](structures/io-counters.md)

### `proseso.getProsesoMemoryaInfo()`

Nagbabalik `Object`:

* `workingSetSize`Integer - Ang halaga ng memorya ay kasalukuyang naka-pin sa aktwal na pisikal na RAM.
* `peakWorkingSetSize` Integer - Ang pinakamataas na halaga ng memorya na hindi pa nai-pin sa aktwal na pisikal RAM.
* `privateBytes` Integer - Ang halaga ng memorya na hindi ibinahagi sa ibang mga proseso, tulad ng JS heap o HTML na nilalaman.
* `sharedBytes` Integer - Ang halaga ng memorya na ibinahagi sa pagitan ng mga proseso, kadalasan ang memorya ay natupok ng Electron code mismo.

Nagbabalik ng mga bagay at nagbibigay ng memoryang paggamit ng istatistika tungkol sa kasalukuyang proseso. Tandaan na ang lahat ng istatistik ay iniulat sa Kilobytes.

### `proseso.getSystemMemoryInfo()`

Nagbabalik `Object`:

* `kabuuan` Integer - Ang kabuuang halaga ng pisikal na memorya sa Kilobytes na maggagamit sa sistema. 
* `libre` Integer - Ang kabuuang halaga ng memorya na hindi nagagamit sa aplikasyon o disk cache.
* `swapTotal` Integer - Ang kabuuang halaga ng mapagpalitang memorya sa Kilobytes ay maggagamit sa sistema. *Windows**Linux*
* `swapFree` Integer - Ang libreng halaga ng pinagpalitang memorya sa Kilobytes na magagamit sa sistema. *Windows**Linux*

Nagbabalik ng bagay at nagbibigay ng memoryang gamit na istatistika tungkol sa buong sistema. Tandaan na ang lahat ng istatistika ay inuulat sa Kilobytes.

### `proseso.hang()`

Dahilan na ang pangunahing thread sa kasalukuyang proseso sabit.

### `proseso.setFdLimit(maxDescriptors)`macOS</em>*Linux*

* `maxDescriptors` Integer

Itakda ang file na tagapaglarawan sa mahinang limitasyon sa `maxDescriptors` o sa OS malakas na limitasyon, alinman ang mas mababa sa kasalukuyang proseso.