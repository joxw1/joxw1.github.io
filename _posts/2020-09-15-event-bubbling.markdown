---
layout: post
title: Event Bubbling - JavaScript
date: 2020-09-15 21:52:13 +0300
description: Was bedeutet Event Bubbling im Kontext vom DOM und JavaScript?
img: bubbles.jpg
fig-caption: Bubbling
tags: [JavaScript, Grundlagen, Events, Event Bubbling, DOM]
---
Ein Thema über was man als JavaScript-Einsteiger nicht nachdenkt ist, wie genau Events abgefeuert werden und wie der Lebenszyklus von Events im DOM ist.

Ein wichtiges Konzept, was man dabei verstehen sollte ist das *_Event Bubbling_*. Übersetzen würden man das Konzept mit "Hochsprudeln von Events". Dabei geht es darum, dass ein Event nicht nur im Ziel-Element (_Target Element_) abgefangen werden kann, sondern auch in allen Eltern-Elementen. 

```html
<main>
  <section>
  	<p>
      Lorem ipsum dolor sit
    </p>
    <p>
      Lorem ipsum dolor sit amet
      <button>
        Link
      </button>
    </p>
  </section>
</main>
```

Wird im oben angegebenen Beispiel ein Event über das `<button>`Element abgefeuert, erreicht das Element auch alle darüber liegenden Elemente. Das heißt, dass ein Event-Handler auch an das `<main>`, `<section>` oder `<p>` Element angehangen werden können.


