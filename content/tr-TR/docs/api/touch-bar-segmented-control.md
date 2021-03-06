## Sınıf: DokunmatikBarParçalıDenetim

> Bir butonun seçili olduğu durumda parçalı bir denetim (bir tuş grubu) oluşturun

İşlem: [Ana](../tutorial/quick-start.md#main-process)

### `new TouchBarSegmentedControl(options)` *Experimental*

* `seçenekler` Nesne 
  * `segmentStyle` Dize - (opsiyonel) Segmentlerin biçimi: 
    * `automatic` - Varsayılan. Parçalı denetimin görünümü denetimin görüntüleneceği pencerenin türüne ve penceredeki konumuna göre otomatik olarak belirlenir.
    * `rounded` - Denetim yuvarlak biçim kullanılarak görüntülenir.
    * `textured-rounded` - Denetim dokulu yuvarlak biçim kullanılarak görüntülenir.
    * `round-rect` - Denetim yuvarlak dik biçim kullanılarak görüntülenir.
    * `textured-square` - Denetim dokulu kare biçim kullanılarak görüntülenir.
    * `capsule` - Denetim kapsül biçimi kullanılarak görüntülenir
    * `small-square` - Denetim küçük kare biçim kullanılarak görüntülenir.
    * `separated` - Denetimdeki segmentler birbirlerine çok yakın fakat değmeden görüntülenir.
  * `mod` Dize - (opsiyonel) Denetimin seçme modu: 
    * `single` - Varsayılan. Bir kerede bir öge seçilir, birini seçmek önceki seçilmiş ögeyi iptal eder.
    * `multiple` - Bir kerede birden fazla öge seçilebilir.
    * `buttons` - Segmentlerin buton gibi davranmasını sağlar, her segment tıklanabilir ve bırakılabilir fakat hiçbir zaman aktif olarak işaretlenmez.
  * `segments` [SegmentedControlSegment[]](structures/segmented-control-segment.md) - Bu denetimin içine yerleştirilmiş bir dizi segment.
  * `selectedIndex` Tamsayı (opsiyonel) - Hali hazırda seçili olan segmentin dizini, kullanıcı etkileşimi ile otomatik olarak güncelleyecek. Mod çoklu olduğunda o son seçilen öge olacak.
  * `change` Fonksiyon - Kullanıcı yeni bir segment seçtiğinde çağırılır 
    * `selectedIndex` Tamsayı - Kullanıcının seçtiği segmentin dizini.
    * `isSelected` Boole - Kullanıcı seçiminin sonucu olarak segmentin seçilip seçilmediği.

### Örnek Özellikler

Aşağıdaki özellikler `TouchBarSegmentedControl` örnekleri olarak uygundur:

#### `touchBarSegmentedControl.segmentStyle`

Denetimin geçerli segment biçimini temsil eden bir `String`. Bu değeri değiştirmek dokunmatik bardaki denetimi hemen güncelleştirir.

#### `touchBarSegmentedControl.segments`

Denetimdeki segmentleri temsil eden bir `SegmentedControlSegment[]` dizisi. Bu değeri değiştirmek dokunmatik bardaki denetimi hemen güncelleştirir. **does not update the touch bar** bu dizideki derin özellikleri güncelleştirir.

#### `touchBarSegmentedControl.selectedIndex`

O anda seçili olan segmenti temsil eden bir `Integer`. Bu değeri değiştirmek dokunmatik bardaki denetimi hemen güncelleştirir. Dokunmatik bar ile olan kullanıcı etkileşimi bu değeri hemen güncelleştirecek.