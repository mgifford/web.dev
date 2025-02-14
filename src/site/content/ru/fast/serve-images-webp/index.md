---
layout: post-old
title: Использование изображений WebP
authors:
  - katiehempenius
description: Изображения WebP занимают меньше места, чем аналогичные изображения в форматах JPEG и PNG, — обычно выигрыш составляет 25–35%. Это позволяет сократить размеры страниц и повысить производительность.
date: 2018-11-05
updated: 2020-04-06
codelabs:
  - codelab-serve-images-webp
tags:
  - performance
feedback:
  - api
---

## Почему это важно?

Изображения WebP занимают меньше места, чем аналогичные изображения в форматах JPEG и PNG, — обычно выигрыш составляет 25–35%. Это позволяет сократить размеры страниц и повысить производительность.

- На YouTube переход на миниатюры в формате WebP позволил [ускорить загрузку страниц на 10%](https://www.youtube.com/watch?v=rqXMwLbYEE4).
- На Facebook переход на WebP позволил [сократить размер файлов](https://code.fb.com/android/improving-facebook-on-android/) на 25-35% по сравнению с JPEG и на 80% по сравнению с PNG.

WebP — отличная замена для изображений JPEG, PNG и GIF. Кроме того, WebP поддерживает сжатие как без потерь, так и с потерями. Сжатие без потерь обеспечивает точную передачу изображения, тогда как сжатие с потерями уменьшает размер файла, но ценой возможного снижения качества изображения.

## Преобразование изображений в WebP

Обычно для преобразования изображений в WebP используется один из следующих инструментов: [инструмент cwebp для командной строки](https://developers.google.com/speed/webp/docs/using) и [плагин WebP для Imagemin](https://github.com/imagemin/imagemin-webp) (пакет npm). Плагин WebP для Imagemin подойдет вам, если ваш проект использует скрипты или инструменты сборки (такие, как Webpack или Gulp), тогда как инструмент для командной строки — оптимальное решение для простых проектов или для ситуаций, когда изображения нужно преобразовать только один раз.

При преобразовании изображений в WebP можно указывать различные параметры сжатия, но в большинстве случаев достаточно указать уровень качества. Уровень качества указывается в виде числа от 0 (наихудшее) до 100 (наилучшее). Поэкспериментируйте с различными уровнями качества, чтобы подобрать оптимальный для ваших потребностей компромисс между качеством изображения и размером файла.

### Использование cwebp

Преобразование отдельного файла с использованием стандартных для cwebp настроек сжатия:

```bash
cwebp images/flower.jpg -o images/flower.webp
```

Преобразование отдельного файла с уровнем качества `50`:

```bash
cwebp -q 50 images/flower.jpg -o images/flower.webp
```

Преобразование всех файлов в каталоге:

```bash
for file in images/*; do cwebp "$file" -o "${file%.*}.webp"; done
```

### Использование Imagemin

Плагин WebP для Imagemin можно использовать как сам по себе, так и в рамках инструментов сборки (Webpack/Gulp/Grunt и т. д.). Обычно это требует добавления в скрипт сборки или файл конфигурации вашего инструмента сборки около 10 строк кода. Вы можете ознакомиться с примерами для [Webpack](https://glitch.com/~webp-webpack), [Gulp](https://glitch.com/~webp-gulp) и [Grunt](https://glitch.com/~webp-grunt).

Если вы не используете ни один из перечисленных инструментов сборки, вы можете использовать Imagemin в качестве самостоятельного скрипта Node. Приведенный ниже скрипт выполняет преобразование файлов в каталоге `images` и сохраняетт их в каталоге `compressed_images`.

```js
const imagemin = require('imagemin');
const imageminWebp = require('imagemin-webp');

imagemin(['images/*'], {
  destination: 'compressed_images',
  plugins: [imageminWebp({quality: 50})]
}).then(() => {
  console.log('Done!');
});
```

## Отображение изображений WebP

Если ваш сайт поддерживает только [браузеры, совместимые с WebP](https://caniuse.com/#search=webp), то вы можете пропустить этот раздел. В противном случае позаботьтесь, чтобы новые браузеры загружали изображения WebP, а старые браузеры использовали резервное изображение:

**Было:**

```html
<img src="flower.jpg" alt="">
```

**Стало:**

```html
<picture>
  <source type="image/webp" srcset="flower.webp">
  <source type="image/jpeg" srcset="flower.jpg">
  <img src="flower.jpg" alt="">
</picture>
```

Для достижения этого результата используется комбинация тегов [`<picture>`](https://developer.mozilla.org/docs/Web/HTML/Element/picture), [`<source>`](https://developer.mozilla.org/docs/Web/HTML/Element/source) и `<img>` с учетом их приоритета по отношению друг к другу.

### picture

Тег `<picture>` используется в качестве обертки для одного тега `<img>` и произвольного (от 0 и больше) количества тегов `<source>`.

### source

Тег `<source>` позволяет указать медиаресурс.

Браузер будет использовать первый источник с подходящим форматом. Если все источники, указанные при помощи тегов `<source>`, имеют неподдерживаемый формат, то происходит загрузка резервного изображения, указанного в теге `<img>`.

{% Aside 'gotchas' %}

- Тег `<source>`, указывающий на изображение в «предпочтительном» формате (в данном случае — WebP), должен быть указан в первую очередь, перед остальными тегами `<source>`.

- Атрибут `type` должен содержать MIME-тип, соответствующий формату изображения. [MIME-тип](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types) изображения и расширение файла зачастую похожи, однако не являются полностью идентичными (например, сравните `.jpg` и `image/jpeg`).

{% endAside %}

### img

Тег `<img>` обеспечивает работу кода в браузерах, которые не поддерживают тег `<picture>`. Браузер без поддержки `<picture>` будет игнорировать «незнакомые» теги, поэтому распознает только тег `<img src="flower.jpg" alt="">` и загрузит соответствующее изображение.

{% Aside 'gotchas' %}

- Тег `<img>` необходимо указывать всегда, и он должен стоять в самом конце, после всех тегов `<source>`.
- Тег `<img>` должен указывать на ресурс в универсально поддерживаемом формате (таком, как JPEG), чтобы его можно было использовать как резервный. {% endAside %}

## Проверка использования WebP

Проверить, что для всех изображений на вашем сайте используется WebP, можно при помощи Lighthouse. Запустите в Lighthouse проверку производительности (**Lighthouse > Options > Performance**) и изучите результаты проверки **Serve images in next-gen formats**. Lighthouse перечислит все изображения, формат которых отличается от WebP.
