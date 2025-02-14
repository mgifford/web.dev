---
layout: post-old
title: Избегайте анимации за пределами этапа компоновки
description: Как пройти проверку «Avoid non-composited animations» в Lighthouse.
date: 2020-08-12
web_lighthouse:
  - non-composited-animations
---

Анимация, выполняемая за пределами этапа компоновки, может выглядеть «рваной» (т. е. не быть плавной) на медленных телефонах или во время выполнения ресурсоемких задач в основном потоке. Дергающиеся анимации могут увеличивать показатель [Cumulative Layout Shift](/cls/) (совокупное смещение макета, CLS) вашей страницы. Снижение CLS помогает улучшить оценку производительности в Lighthouse.

## Предыстория

Алгоритмы, используемые браузером для преобразования HTML, CSS и JavaScript в растровое изображение, в совокупности известны как *конвейер рендеринга*.

<figure class="w-figure"> {% Img src="image/tcFciHGuF3MxnTr1y5ue01OGLBn2/68xt0KeUvOpB8kA1OH0a.jpg", alt="Конвейер рендеринга состоит из следующих последовательных этапов: JavaScript, стилизация, макет, отрисовка, компоновка.", width="800", height="122" %} <figcaption class="w-figcaption">Конвейер рендеринга.</figcaption></figure>

Ничего страшного, если вы не понимаете, за что отвечают этапы рендеринга. Важно лишь понимать, что на каждом этапе рендеринга браузер обрабатывает данные, полученные в ходе предыдущего этапа. Например, если ваш код инициирует изменения на этапе макета, это приводит к необходимости повторного запуска отрисовки и компоновки. Анимация за пределами этапа компоновки — это любая анимация, которая запускает один из предыдущих этапов рендеринга (стилизацию, макет или отрисовку). Анимации за пределами этапа компоновки имеют низкую производительность, так как заставляют браузер совершать больше работы.

Чтобы подробнее узнать о конвейере рендеринга, ознакомьтесь со следующими ресурсами:

- [Взгляд изнутри на современные веб-браузеры (часть 3)](https://developers.google.com/web/updates/2018/09/inside-browser-part3)
- [Упрощение и сокращение областей прорисовки](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
- [Используйте свойства, вызывающие только компоновку, и контролируйте количество слоев](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)

## Как Lighthouse обнаруживает анимации за пределами этапа компоновки

Когда анимацию невозможно выполнить в рамках этапа компоновки, Chrome сообщает о причинах в трассировке DevTools, которую и использует Lighthouse. Lighthouse перечисляет узлы DOM, анимация которых выполняется за пределами этапа компоновки, а также указывает причины такого поведения для каждой анимации.

## Как обеспечить анимацию в рамках этапа компоновки

См. статьи [Используйте свойства, вызывающие только компоновку, и контролируйте количество слоев](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count) и [Высокопроизводительная анимация](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/).

## Ресурсы

- [Исходный код проверки *Avoid non-composited animations*](https://github.com/GoogleChrome/lighthouse/blob/master/lighthouse-core/audits/non-composited-animations.js)
- [Используйте свойства, вызывающие только компоновку, и контролируйте количество слоев](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
- [Высокопроизводительная анимация](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)
- [Упрощение и сокращение областей прорисовки](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
- [Взгляд изнутри на современные веб-браузеры (часть 3)](https://developers.google.com/web/updates/2018/09/inside-browser-part3)
