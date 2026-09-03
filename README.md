# E3 Interaction Demo

Figma загварыг HTML, CSS болгон угсарсан статик сургалтын төсөл. Одоогийн хувилбарт JavaScript болон motion animation **санаатайгаар оруулаагүй**. Бүх зураг, icon-ыг Figma-аас татаж `assets/` дотор локал байдлаар ашигласан.

- Figma source: [01 Ai web Slides — Home Desktop](https://www.figma.com/design/VITeKdA9ofn2MXsF0sLghy/01-Ai-web--Slides-?node-id=2638-1690)
- Гол файлууд: `index.html`, `styles.css`
- Motion target-ууд: HTML доторх `data-motion-target` attribute
- Dependency: байхгүй

## Ажиллуулах

`index.html` файлыг browser-оор шууд нээж болно. Local server ашиглах бол:

```bash
npx serve .
```

эсвэл VS Code-ийн Live Server extension ашиглана.

## Motion нэмэхээс өмнөх дүрэм

Доорх prompt бүрийг **тус тусад нь** туршина. Эхлэх бүрдээ одоогийн статик хувилбараас шинэ branch үүсгэх нь харьцуулахад хялбар.

- Figma-ийн layout, хэмжээ, spacing, өнгө, typography-г өөрчлөхгүй.
- Motion нэмэхдээ `data-motion-target`-аар заасан element-үүдийг ашиглана.
- `prefers-reduced-motion: reduce` үед хөдөлгөөнгүй, бүрэн ашиглах боломжтой байна.
- Keyboard focus, touch device, loading/error state-ийг орхихгүй.
- Animation нь layout shift үүсгэхгүй; зөвхөн `transform` болон `opacity`-г голчлон ашиглана.
- Нэг prompt-ийг туршихдаа бусад motion effect-ийг зэрэг нэмэхгүй.

## 1. Magnetic Button

Хаана тохиромжтой: navbar-ийн “Жолоодож үзэх” CTA дээр хамгийн цэвэр харагдана. Hero-ийн үндсэн CTA дээр нэмж туршиж болох ч бүх жижиг товчинд зэрэг ашиглавал анхаарал сарнина.

Target: `[data-motion-target~="magnetic-button"]`

```text
Энэ static HTML/CSS хуудасны одоогийн visual design болон layout-ийг өөрчлөхгүйгээр цэвэрхэн, нарийн мэдрэмжтэй Magnetic Button interaction нэм. Зөвхөн `[data-motion-target~="magnetic-button"]` selector-т тохирох element-үүдэд үйлчилнэ. Dependency ашиглахгүй, vanilla JavaScript болон CSS-ээр хэрэгжүүл. Fine pointer бүхий cursor товчны influence area-д ороход товчийг pointer руу үл ялиг татаж, X болон Y тэнхлэг тус бүрийн шилжилтийг хамгийн ихдээ 8px-ээр хязгаарла. Depth мэдрэмж үүсгэхийн тулд товчны label-ийг товчны шилжилтийн 40%-тай тэнцэх хэмжээгээр хөдөлгө. Зөвхөн `requestAnimationFrame` болон `transform` ашигла. `pointerleave` үед товчийг 420–520ms хугацаанд зөөлөн overshoot-тэй, давтагдсан bounce-гүйгээр яг анхны байрлалд нь spring хөдөлгөөнөөр буцаа. Touch болон coarse-pointer device дээр effect-ийг ажиллуулахгүй. Click behavior, keyboard focus, accessible name болон одоогийн хэмжээсүүдийг хэвээр хадгал. `prefers-reduced-motion: reduce` үед бүх motion-ийг идэвхгүй болго. Implementation-ийг тусгаарласан байлгаж, түүнийг арилгахад анхны static хуудас яг хэвээр сэргэдэг болго.
```

## 2. Spotlight Hover Effect

Хаана тохиромжтой: product card болон Current Offers / Inventory card. Зургийг дарахгүй, зөвхөн cursor орчимд маш зөөлөн гэрлийн halo үүсгэвэл dark photo card дээр илүү сайн мэдрэгдэнэ.

Target: `[data-motion-target~="spotlight-hover"]`

```text
Одоо байгаа `[data-motion-target~="spotlight-hover"]` selector-т тохирох card-уудад зөөлөн Spotlight Hover Effect нэм. Зөвхөн vanilla JavaScript болон CSS ашигла. Card-ын markup, хэмжээ, image crop, text color болон default static харагдацыг өөрчлөхгүй. Идэвхтэй card тус бүртэй харьцуулсан pointer position-ийг тооцоолж, normalize хийсэн утгуудыг `--spot-x`, `--spot-y` CSS custom property-оор дамжуул. Spotlight-ийг `pseudo-element` ашиглан render хий: pointer дээр төвлөсөн зөөлөн `radial-gradient`, image card дээр хамгийн ихдээ `opacity: 0.16`, light card дээр `opacity: 0.08`, ойролцоогоор 260px radius, `pointer-events: none` байх бөгөөд одоогийн card boundary дотор бүрэн clip хийгдэнэ. Effect-ийг 180ms хугацаанд fade in, 300ms хугацаанд fade out болго. Update-ийг `requestAnimationFrame`-ээр хийж, нэг frame дотор layout read болон write-ийг олон дахин бүү ажиллуул. Зөвхөн cursor байрлаж буй нэг card-ыг идэвхжүүл. Touch/coarse pointer болон `prefers-reduced-motion: reduce` үед анхны static design-ийг өөрчлөхгүй үлдээ. Text contrast-ийг хадгалж, харагдахуйц нэмэлт border эсвэл байнгын glow бүү үүсгэ.
```

## 3. Morphing Tabs & Shared Layout

Хаана тохиромжтой: Inventory card-ийн “New / Pre-Owned” сонголт. Shared pill background хоёр tab-ын хооронд шилжвэл ойлгомжтой, жижиг хүрээнд motion concept-ийг сайн харуулна.

Target: `[data-motion-target~="morphing-tabs-shared-layout"]`

```text
`[data-motion-target~="morphing-tabs-shared-layout"]` доторх Inventory-ийн одоогийн хоёр button-ийг shared-layout active indicator бүхий accessible Morphing Tabs болго. Framework болон external animation library ашиглахгүй, зөвхөн vanilla JavaScript болон CSS-ээр хэрэгжүүл. Card-ын яг одоогийн хэмжээ болон visual language-ийг хэвээр хадгал. Зөв `role="tablist"`, `role="tab"`, `aria-selected`, `tabindex` attribute-ууд болон `Left`, `Right`, `Home`, `End` keyboard navigation нэм. Сонгогдсон tab-ын хэмжээг тооцдог нэг shared white active-pill indicator үүсгэж, `width` эсвэл `left`-ийн layout animation ашиглахгүйгээр `translate` болон `scale`-аар tab-уудын хооронд morph хий. `cubic-bezier(.22,1,.36,1)` шиг зөөлөн easing бүхий 360–440ms transition ашигла. Сонгогдсон label-ийн emphasis-ийг 140–180ms хугацаанд crossfade хий. Default сонголтыг `New` болгоод click болон keyboard selection бүр indicator-ийг шинэчилдэг болго. Одоогоор vehicle image шинээр зохиох эсвэл солихгүй; энэ туршилтаар зөвхөн shared-layout motion-ийг үнэлнэ. `ResizeObserver` ашиглан хэмжээг аюулгүй дахин тооцоол. `prefers-reduced-motion: reduce` үед сонголтыг motion-гүйгээр шууд соль. Focus ring-ийг харагдахуйц хэвээр үлдээж, эргэн тойрны content-ийг бүү хөдөлгө.
```

## 4. 3D Tilt Cards

Хаана тохиромжтой: Vehicles болон Energy хэсгийн том product card-ууд. Том зурагтай тул бага өнцгийн perspective depth ойлгомжтой харагдана. Жижиг info card дээр ашиглахгүй.

Target: `[data-motion-target~="3d-tilt-card"]`

```text
Энэ static хуудасны `[data-motion-target~="3d-tilt-card"]` selector-т тохирох card-уудад premium мэдрэмжтэй боловч хэтрүүлээгүй 3D Tilt interaction нэм. Зөвхөн vanilla JavaScript болон CSS ашигла. Card бүрийн анхны хэмжээ, crop, border radius, content position болон spacing-ийг хэвээр хадгал. Тогтвортой parent element дээр `perspective` тохируулж, pointer position-д үндэслэн card-ыг эргүүл. X тэнхлэгийн эргэлтийг 3.5deg, Y тэнхлэгийн эргэлтийг 4.5deg-ээс хэтрүүлэхгүй. `translateZ`-ийг зөвхөн доторх decorative layer-уудад хэрэглэ. Ирмэгийн хоосон зай ил гарахаас сэргийлж image-ийг хамгийн ихдээ `scale(1.018)` хүртэл томруулж болно. Text болон button-уудын 2D байрлалыг өөрчлөхгүйгээр Z тэнхлэгт 10–14px өргөж болно. Утгуудыг `requestAnimationFrame` ашиглан interpolate хийж, зөөлөн жинтэй мэдрэмж үүсгэ. `pointerleave` үед ойролцоогоор 500ms хугацаанд бүх утгыг тэг рүү буцаа. Tilt effect-ийг page scrolling input-тэй хэзээ ч бүү холбо, click event-ийг бүү саатуул. Зөвхөн hover дэмждэг fine pointer дээр идэвхжүүл. `prefers-reduced-motion: reduce`, touch device болон keyboard focus card дотор байрлаж байх үед effect-ийг идэвхгүй болго. Хэт их shadow бүү хэрэглэ; шаардлагатай бол зөөлөн, 18%-иас бага opacity-той байлга. Static resting state нь pixel түвшинд анхны харагдацтай ижил байх ёстой.
```

## 5. Seamless Button Loading States

Хаана тохиромжтой: Hero-ийн “Одоо захиалах” болон FSD-ийн “Жолоодож үзэх цаг авах” action. Худалдан авалт/захиалгын үйлдэлд idle → loading → success → idle урсгал хамгийн ойлгомжтой.

Target: `[data-motion-target~="seamless-button-loading"]`

```text
`[data-motion-target~="seamless-button-loading"]` selector-т тохирох button-уудад зориулсан тусгаарлагдсан Seamless Loading State demo-г vanilla JavaScript болон CSS ашиглан хэрэгжүүл. Одоогийн button-ын resting width, height, color, radius болон typography-г яг хэвээр хадгал. Button идэвхжихэд давхар submission-оос сэргийлж, `aria-busy="true"` тохируул. Button-ын width-ийг урьдчилан хэмжсэн idle width дээр нь lock хий. Анхны label-ийг 4px дээш slide хийхийн зэрэгцээ crossfade хийгээд, жижиг CSS spinner болон “Түр хүлээнэ үү” label харуул. 1400ms-ийн дараа үйлдэл дууссан мэт simulate хийж, checkmark болон “Амжилттай” төлөв рүү morph хийн 1000ms харуулсны дараа анхны label болон enabled state рүү цэвэрхэн буцаа. 180–240ms орчим `opacity` болон `transform` transition ашигла. Content-ийг гэнэт сольж эсвэл layout shift үүсгэж болохгүй. Keyboard activation болон visible focus-ийг хэвээр хадгал. Status өөрчлөлтийг давхардуулалгүйгээр нэг `aria-live="polite"` region-оор зарла. `prefers-reduced-motion: reduce` идэвхтэй үед ижил timing болон semantic-ийг хадгалах боловч state-үүдийг motion-гүйгээр шууд соль. Logic-ийг дараа нь бодит `async request`-ээр хялбар солих боломжтой байлга.
```

## 6. Staggered Text Scramble / Reveal

Хаана тохиромжтой: эхний hero-ийн “Model 3” гарчиг/дэд текст дээр нэг удаа, дараа нь product card-ын гарчгууд viewport-д ороход. Paragraph бүрийг scramble хийхгүй — уншигдах чадвар муудна.

Target: `[data-motion-target~="staggered-text-reveal"]`

```text
`[data-motion-target~="staggered-text-reveal"]` доторх element-үүдэд маш богино scramble phase бүхий гоёмсог Staggered Text Reveal нэм. Зөвхөн vanilla JavaScript болон CSS ашигла. Эцсийн text, markup-ийн утга, font metrics, line break болон container dimensions-ийг яг хэвээр хадгал. Element viewport-д 30% threshold-оор анх орж ирэхэд шууд child болох heading, eyebrow, subtitle text-үүдийг унших дарааллаар reveal хий. Эхлээд `opacity: 0`, `translateY(14px)` төлөвөөс эхэлж, `cubic-bezier(.22,1,.36,1)` easing ашиглан 600ms хугацаанд static төлөвт оруул. Element бүрийг 90ms stagger-тай эхлүүл. Зөвхөн эхний 220–300ms хугацаанд space биш character-уудын хамгийн ихдээ 35%-ийг төвийг сахисан Latin letter эсвэл number-оор түр сольж, жинхэнэ character-уудыг зүүнээс баруун тийш аажмаар сэргээ. Монгол Cyrillic CTA label, price агуулсан number болон screen-reader text-ийг хэзээ ч scramble хийхгүй. Original text-ийг аюулгүй хадгалж, group бүрийг зөвхөн нэг удаа ажиллуулан `IntersectionObserver` ашигла. Assistive technology үргэлж эцсийн text-ийг уншихын тулд шаардлагатай хэсэгт `aria-label` эсвэл visually hidden тогтвортой copy нэм. `prefers-reduced-motion: reduce` үед scramble болон translation ашиглалгүйгээр эцсийн text-ийг шууд харуул. Layout shift үүсгэхгүй бөгөөд зайлшгүй шаардлагагүй бол үгсийг тус тусын DOM node болгон бүү задал.
```

## 7. Progressive / Smooth Scroll Progress

Хаана тохиромжтой: бүх хуудасны дээд ирмэгт 2–3px progress line. Урт landing page учраас хэрэглэгч хаана явж байгаагаа мэднэ. Native scroll-ийг хүчээр удаашруулахгүй; progress smoothing л хийнэ.

Target: `body` болон `[data-motion-target="scroll-progress-anchor"]`

```text
Энэ урт static landing page-д зориулсан minimal Scroll Progress indicator-ийг vanilla JavaScript болон CSS ашиглан нэм. Viewport-ийн хамгийн дээд ирмэгт, бүх page content-ийн дээр байрлах, interaction авахгүй 3px өндөртэй нэг fixed bar үүсгэ. `transform-origin: left` тохируулж, одоогийн royal blue `#4e6cda` өнгийг ашигла. Progress утгыг `scrollY`-г тухайн үеийн scroll хийж болох document height-д хувааж тооцоод 0–1 хооронд clamp хий. Зөвхөн `transform: scaleX()` ашиглан render хий. Харагдах утгыг бодит progress руу `requestAnimationFrame` interpolation ашиглан зөөлрүүл, гэхдээ ойролцоогоор 100ms-ээс их хоцролт үүсгэж болохгүй. Resize, image load болон document height өөрчлөгдөх бүрд `ResizeObserver` ашиглан зөв дахин тооцоол. Native scrolling-ийг өөрчлөх эсвэл hijack хийхгүй, custom scroll container нэмэхгүй, anchor behavior-ийг өөрчлөхгүй. Progress bar нь pointer event авахгүй бөгөөд layout shift үүсгэхгүй байх ёстой. `prefers-reduced-motion: reduce` үед interpolation ашиглалгүйгээр bar-ын утгыг шууд шинэчил. Code-ийг тусгаарласан байлгаж, нэгээс олон удаа initialize хийгдвэл өмнөх observer болон listener-үүдийг бүрэн clean up хий.
```

## Туршилтын санал болгосон дараалал

1. Progressive Scroll Progress — хамгийн бага эрсдэлтэй, бүтэн хуудсын уртыг ашиглана.
2. Magnetic Button — жижиг scope-той pointer interaction.
3. Spotlight Hover — card бүрийн cursor coordinate ойлгоход тохиромжтой.
4. Morphing Tabs — state, accessibility, shared indicator дадлага.
5. Seamless Loading — async UI state болон aria-live дадлага.
6. 3D Tilt — transform math, device condition, performance дадлага.
7. Text Scramble / Reveal — DOM text болон accessibility хамгийн нарийн тул хамгийн сүүлд.

## Файлын бүтэц

```text
e3-interaction-demo/
├── assets/             # Figma-аас татсан PNG, SVG
├── index.html          # Семантик статик бүтэц + motion target hooks
├── styles.css          # Figma-д суурилсан responsive styling
└── README.md           # Motion analysis, prompt-ууд
```
