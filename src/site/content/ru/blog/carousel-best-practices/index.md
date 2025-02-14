---
layout: post
title: Рекомендации по работе с каруселями
subhead: Повысьте удобство использования и производительность, оптимизировав карусели.
authors:
  - katiehempenius
description: Узнайте, как повысить удобство использования и производительность, оптимизировав карусели.
date: 2021-01-26
hero: image/admin/i7tjE04MYo7xJOZKkyQI.jpg
tags:
  - blog
  - performance
  - web-vitals
---

Карусель — это компонент пользовательского интерфейса, отображающий контент в виде слайд-шоу. Слайды карусели можно переключать как автоматически, так и вручную. Карусели можно применять и для других целей, но чаще всего их используют для показа изображений, продуктов и рекламных акций на начальных страницах.

В этой статье рассказывается, как повысить производительность каруселей и удобство их использования.

<figure class="w-figure">{% Img src="image/admin/u2FlXalClwBeDOBBiwxu.png", alt="Изображение, на котором показана карусель", width="629", height="420", class="w-screenshot" %}</figure>

## Производительность

Хорошо реализованная карусель должна оказывать минимальное или нулевое влияние на производительность. Но в каруселях часто содержатся медиаресурсы большого размера. Они могут снижать производительность независимо от того, где они отображаются: в карусели или где-то еще.

- **LCP (Largest Contentful Paint, самый крупный отображаемый элемент содержимого)**

    В больших каруселях, расположенных в пределах экрана, часто содержится самый крупный отображаемый элемент содержимого (LCP) страницы. Соответственно, такие карусели могут значительно влиять на метрику LCP. Оптимизировав карусель в подобных случаях, можно значительно улучшить метрику LCP. Подробный рассказ о принципах измерения метрики LCP на страницах с каруселями см. в разделе «[Измерение метрики LCP для каруселей](#lcp-measurement-for-carousels)».

- **FID (First Input Delay, время ожидания до первого взаимодействия с контентом)**

    Карусели предъявляют минимальные требования к JavaScript и поэтому не должны влиять на интерактивность страницы. Если вы обнаружите, что в карусели вашего сайта есть долго работающие скрипты, рекомендуется перейти на другие средства работы с каруселями.

- **CLS (Cumulative Layout Shift, совокупное изменение макета)**

    Неожиданно большое количество каруселей содержит бесполезные несоставные анимации, которые могут увеличивать метрику CLS. Из-за этого на страницах с автоматически воспроизводимыми каруселями метрика CLS может иметь бесконечное значение. В таких случаях совокупное изменение макета обычно незаметно человеческому глазу, поэтому легко пропустить его. Во избежание этой проблемы [не используйте несоставные анимации](/non-composited-animations/) в каруселях (например, во время переходов между слайдами).

## Рекомендации по повышению производительности

### Загрузка содержимого карусели с использованием HTML

Содержимое карусели следует загружать, используя HTML-разметку страницы. В этом случае браузер сможет обнаружить его на ранних этапах загрузки страницы. Запускать загрузку содержимого карусели с помощью JavaScript, вероятно, самая большая ошибка, которая снижает производительность и которой следует избегать при использовании карусели. Такой подход замедляет загрузку изображений и может ухудшить метрику LCP.

{% Compare 'better' %}

```html
<div class="slides">
  <img src="https://example.com/cat1.jpg">
  <img src="https://example.com/cat2.jpg">
  <img src="https://example.com/cat3.jpg">
</div>
```

{% endCompare %}

{% Compare 'worse' %}

```javascript
const slides = document.querySelector(".slides");
const newSlide = document.createElement("img");
newSlide.src = "htttp://example.com/cat1.jpg";
slides.appendChild(newSlide);
```

{% endCompare %}

Чтобы еще больше оптимизировать карусель, можно загружать первый слайд статически, а затем постепенного добавлять в него элементы управления навигацией и дополнительный контент. Этот метод лучше всего подходит для случаев, когда вы располагаете длительным вниманием пользователя, — вы получаете дополнительное время для загрузки контента. В других случаях, например на начальных страницах, на которых пользователи задерживаются только на секунду-две, аналогичную эффективность дает только загрузка одного изображения.

### Избегайте сдвигов макета

{% Aside %}

В Chrome версий 88–90 [исправлено много ошибок](https://chromium.googlesource.com/chromium/src/+/master/docs/speed/metrics_changelog/cls.md), связанных порядком расчета сдвигов макета. Многие из этих исправлений связаны с каруселями. В результате в последних версиях Chrome показатели сдвигов макета, связанных с каруселями, для сайтов должны уменьшиться.

{% endAside %}

Элементы управления навигацией и переходами между слайдами — самые распространенные причины сдвигов макета в каруселях.

- **Переходы между слайдами:** сдвиги макета, которые происходят во время переходов между слайдами, обычно связаны с обновлением свойств элементов DOM, влияющих на макет. Вот примеры таких свойств: `left`, `top`, `width` и `marginTop`. Чтобы не допустить сдвиги макета, для перехода между этими элементами используйте [свойство `transform`](https://developer.mozilla.org/docs/Web/CSS/transform). В [этой демонстрации](https://glitch.com/~basic-carousel) показано, как создать карусель с базовыми возможностями с использованием свойства `transform`.

- **Элементы управления навигацией:** перемещение, добавление или удаление элементов управления навигацией по карусели из модели DOM может привести к сдвигам макета в зависимости от того, как эти изменения реализованы. В каруселях с такой проблемой это обычно происходит, когда пользователь наводит на них указатель мыши.

Ниже приведено несколько общих моментов, вызывающих путаницу при измерении метрики CLS для каруселей.

- **Карусели с автоматическим воспроизведением:** переходы между слайдами — самая распространенная причина сдвигов макета, связанных с каруселями. В каруселях без автоматического воспроизведения такие сдвиги макета обычно происходят в пределах 500 мс после взаимодействия с пользователем и, [следовательно, не учитываются в метрике Cumulative Layout Shift (CLS)](/cls/#expected-vs.-unexpected-layout-shifts). Для каруселей с автоматическим воспроизведением такие сдвиги макета могут не только учитываться в метрике CLS, но и повторяться бесконечно. Таким образом, очень важно убедиться, что карусель с автоматическим воспроизведением не является причиной сдвигов макета.

- **Прокрутка:** в некоторых каруселях пользователи могут переходить по слайдам карусели, используя функцию прокрутки. Если начальная позиция элемента изменяется и при этом смещение элемента при прокрутке (то есть [`scrollLeft`](https://developer.mozilla.org/docs/Web/API/Element/scrollLeft) или [`scrollTop`](https://developer.mozilla.org/docs/Web/API/Element/scrollTop)) изменяется на ту же величину, но в противоположном направлении, это не считается сдвигом макета при условии, что это все происходит в одном фрейме.

Дополнительные сведения о сдвигах макета см. в статье «[Отладка при сдвигах макета](/debug-layout-shifts/#identifying-the-cause-of-a-layout-shift)».

### Используйте современные технологии

На многих сайтах карусели реализованы с помощью [сторонних библиотек JavaScript](/third-party-javascript). Если вы используете старые средства для работы с каруселями, можно повысить производительность, перейдя на новые средства. В новых средствах, как правило, используются более эффективные API и меньше вероятность того, что потребуются дополнительные зависимости, например jQuery.

Однако (в зависимости от типа создаваемой карусели) вам может вообще не понадобиться JavaScript. Новый API [Scroll Snap](https://developer.mozilla.org/docs/Web/CSS/CSS_Scroll_Snap) позволяет реализовать переходы, похожие на переходы в каруселях, используя только HTML и CSS.

Ниже указано несколько ресурсов по использованию `scroll-snap`, которые могут быть вам полезны.

- [Создание компонента Stories (web.dev)](/building-a-stories-component/)
- [Веб-стиль нового поколения: scroll snap (web.dev)](/next-gen-css-2019/#scroll-snap)
- [Карусель с использованием только CSS (хитрости CSS)](https://css-tricks.com/css-only-carousel/)
- [Как создать карусель с использованием только CSS (хитрости CSS)](https://css-tricks.com/how-to-make-a-css-only-carousel/)

### Оптимизируйте контент карусели

В каруселях часто содержатся самые крупные изображения на сайте, поэтому стоит потратить время и проверить, полностью ли оптимизированы эти изображения. Выбор правильного формата изображения и уровня сжатия, [использование CDN изображений](/image-cdns) и [srcset для передачи нескольких версий изображений](https://developer.mozilla.org/docs/Web/CSS/CSS_Scroll_Snap) — методы, с помощью которых можно уменьшить размер передаваемых изображений.

## Измерение производительности

В этом разделе рассказывается об измерении метрики LCP применительно к каруселям. Несмотря на то что во время расчета метрики LCP карусели обрабатываются точно так же, как любые другие элементы пользовательского интерфейса, механизм расчета метрики LCP для каруселей с автоматическим воспроизведением часто приводит к путанице.

### Измерение метрики LCP для каруселей

Ниже изложены ключевые моменты, позволяющие понять принцип расчета метрики LCP для каруселей.

- При расчете метрики LCP элементы страницы учитываются по мере их отображения во фрейме. После того как пользователь начинает взаимодействовать со страницей (касаться ее элементов, прокручивать содержимое или нажимать клавиши), новые элементы не больше не учитываются при расчете метрики LCP. Таким образом, любой слайд в карусели с автоматическим воспроизведением может стать последним самым крупным отображаемым элементом содержимого, тогда как в статической карусели таким элементом может быть только первый слайд.
- При рендеринге двух изображений одинакового размера самым крупным отображаемым элементом содержимого будет считаться первое изображение. Самый крупный отображаемый элемент содержимого изменяется только тогда, когда появляется новый элемент, который больше текущего самого крупного отображаемого элемента содержимого. Таким образом, если все элементы карусели имеют одинаковый размер, самым крупным отображаемым элементом содержимого должно быть первое отображаемое изображение.
- В процессе оценки элементов при расчете метрики LCP учитывается «[видимый или внутренний размер в зависимости от того, какой из них меньше](/lcp)». Таким образом, если в карусели с автоматическим воспроизведением отображаются изображения одинакового размера, но с разными [внутренними размерами](https://developer.mozilla.org/docs/Glossary/Intrinsic_Size), которые меньше размера дисплея, то по мере отображения новых слайдов самый крупный отображаемый элемент содержимого может изменяться. В этом случае, если все изображения отображаются с одинаковым размером, изображение с наибольшим внутренним размером будет считаться самым крупным отображаемым элементом содержимого. Чтобы метрика LCP имела малое значение, все элементы в карусели с автоматическим воспроизведением должны иметь одинаковый внутренний размер.

### Изменения порядка расчета метрики LCP для каруселей в Chrome 88

Начиная с [Chrome 88](https://chromium.googlesource.com/chromium/src/+/master/docs/speed/metrics_changelog/2020_11_lcp.md), при определении самого крупного отображаемого элемента содержимого принимаются во внимание изображения, которые позже удаляются из DOM. Ранее такие изображения не учитывались. Для сайтов, на которых используются карусели с автоматическим воспроизведением, изменение этого определения окажет нейтральное или положительное влияние на оценки LCP.

Это изменение было внесено по результатам [наблюдений](https://github.com/anniesullie/LCP_Examples/tree/master/removed_from_dom), согласно которым на многих сайтах переходы по слайдам карусели реализованы путем удаления ранее отображаемого изображения из дерева DOM. До Chrome 88 каждый раз, когда отображался новый слайд, удаление предыдущего элемента приводило к пересчету метрики LCP. Это изменение влияет только на карусели с автоматическим воспроизведением, так как по определению самый крупный отображаемый элемент содержимого может появиться только до первого взаимодействия пользователя со страницей.

## Прочие соображения

В этом разделе изложены рекомендации по пользовательским интерфейсам и продуктам, которые следует учитывать при реализации каруселей. Карусели должны помогать в достижении бизнес-целей и отображать контент в удобной для навигации и чтения форме.

### Рекомендации по навигации

#### Используйте заметные элементы управления навигацией

Элементы управления навигацией по карусели должны быть хорошо заметны и их должно быть удобно нажимать. Это редко удается сделать хорошо — в большинстве каруселей элементы управления навигацией маленькие и тонкие. Имейте в виду, что один и тот же цвет или стиль элементов управления навигацией редко подходит для всех ситуаций. Например, стрелка, которая хорошо видна на темном фоне, может быть незаметна на светлом изображении.

#### Отображайте ход навигации

Элементы управления навигацией по карусели должны давать представление об общем количестве слайдов и о том, в каком месте карусели находится пользователь. Так пользователю будет проще переходить к определенному слайду и понимать, какой контент он уже просмотрел. В некоторых ситуациях возможность предварительно просмотреть последующий контент (например, фрагмент из следующего слайда или список эскизов) может быть полезной и повысить заинтересованность пользователя.

#### Поддержка жестов на мобильных устройствах

На мобильных устройствах помимо традиционных элементов управления навигацией (например, экранных кнопок) следует поддерживать жесты смахивания.

#### Обеспечьте альтернативные пути навигации

Маловероятно, что большинство пользователей будет взаимодействовать со всем контентом карусели, поэтому контент, на который ссылаются слайды карусели, должен быть доступен через другие пути навигации.

### Рекомендации по обеспечению удобочитаемости

#### Не используйте автоматическое воспроизведение

Автоматическое воспроизведение создает две почти парадоксальные проблемы: экранные анимации отвлекают пользователей и уводят взгляд от более важного контента. Кроме того, поскольку пользователи часто связывают анимацию с рекламой, они игнорируют карусели, которые воспроизводятся автоматически.

Таким образом, автоматическое воспроизведение редко бывает полезно. Если важен контент, отказ от автоматического воспроизведения делает его максимально доступным; если контент карусели не важен, то автоматическое воспроизведение отвлечет внимание пользователя от более важного содержимого. Кроме того, содержимое слайдов в каруселях с автоматическим воспроизведением может быть трудно читать (и это тоже раздражает). Люди читают с разной скоростью, поэтому редко бывает, что слайды карусели переключаются в «правильное» время для разных пользователей.

В идеальном случае переходами по слайдам должен управлять пользователь с помощью элементов управления. Если необходимо использовать автоматическое воспроизведение, оно должно отключаться при наведении указателя мыши на карусель. Кроме того, скорость перехода между слайдами должна зависеть от содержимого слайда — чем больше текста на слайде, тем дольше он должен отображаться на экране.

#### Не смешивайте текст и изображения

Часто текстовое содержимое карусели добавляют в соответствующий файл изображения, а не отображают отдельно с помощью разметки HTML. Такой подход плох с точки зрения поддержки специальных возможностей, локализации и степени сжатия. Он также поощряет использовать подход «один размер для всех случаев» при создании ресурсов. Однако один и тот же формат изображения и текста редко бывает одинаково удобно читать на компьютерах и мобильных устройствах.

#### Будьте краткими

У вас есть лишь доля секунды, чтобы привлечь внимание пользователя. Короткий текст по существу увеличивает шансы донести нужное сообщение до пользователя.

### Рекомендации по продуктам

Карусели хороши, когда невозможно использовать дополнительное вертикальное пространство для отображения дополнительного контента. Зачастую карусели на страницах продуктов — хороший пример этого сценария использования.

Тем не менее карусели не всегда используются эффективно.

- Пользователя легко [принимают](https://www.nngroup.com/articles/auto-forwarding/) карусели за рекламу (особенно если в них содержатся рекламные объявления или они автоматически прокручиваются). Пользователи склонны игнорировать рекламу — это явление называют [баннерной слепотой](https://www.nngroup.com/articles/banner-blindness-old-and-new-findings/).
- Карусели часто используются, чтобы «примирить» несколько отделов организации и не принимать решения о бизнес-приоритетах. В результате карусели могут легко превратиться в «свалку» неэффективного контента.

#### Проверяйте свои предположения

Влияние каруселей на бизнес, особенно на домашних страницах, необходимо оценивать и проверять. С помощью показателей CTR карусели можно определить, эффективны ли карусель и ее содержимое.

#### Будьте актуальными

Карусели максимально эффективны, если они содержат интересный и релевантный контент, представленный в понятном контексте. Если контент не привлекает пользователя за пределами карусели, то разместив его в карусели, вы не повысите его эффективность. Если вам необходимо использовать карусель, расставьте приоритеты для контента и сделайте так, чтобы каждый слайд был достаточно релевантным, и пользователь захотел перейти к следующему слайду.
