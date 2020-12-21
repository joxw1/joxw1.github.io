---
layout: post
title: CSS - Overlay mit Hilfsklassen
date: 2020-12-21 23:43:33 +0300
description: Vorstellung eines Blogpost für Overlay-Hilfsklassen.
img: overlay.jpg
fig-caption: Overlay
tags: [CSS, Utility, Hilfsklasse, Overlay, SCSS]
---
Über meine Nummer-1 Goto-Seite [FrontEndFront](https://frontendfront.com/) bin ich über einen [Blog-Post](https://css-irl.info/a-utility-class-for-covering-elements/) gestolpert, der erarbeitet, wie man in CSS eine Hilfsklasse implementieren könnte, die ein Overlay erzeugt. Ich fand das Thema ganz cool, also habe ich mir überlegt den Post hierher zu portieren und für mich aufzuarbeiten.

### Die Herausforderung

Der Autor des Blog-Posts leitet das Problem ein, indem er angibt, wie er klassischerweise zwei Elemente übereinander darstellt. Dabei muss das Elternelement eine relative Position haben und das Kindelement auf absolut eingestellt sein.

```css
.original-element {
  position: relative;
}

.covering-element {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
```
<br>
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="BaLdGEb" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Covering element simple">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/BaLdGEb">
  Covering element simple</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Dieses "Überlappen" der Elemente kann repetitiv sein, weshalb der Autor sich überlegt hat, Hilfsklassen zu definieren. Diese werden nachfolgend vorgestellt.

#### Hilfsklassen

Hilfsklassen sind CSS-Klassen, die nur einen einzelnen Zweck erfüllen. Das kann z.B. eine Abstandsdefinition zum nächsten Element sein oder das Zentrieren eines Textes. Sie können extrem hilfreich sein, wenn man sich oft wiederholende Aufgaben in CSS definieren muss. 

Für das Überlappen von Elementen könnte die Hilfsklasse folgendermaßen aussehen.

```css
.overlay {
  position: absolute;
  content: '';
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
```

Auf den ersten Blick sieht dies sehr sinnvoll aus, da man diese Klasse an jedes Element hinzufügen kann, welches ein Overlay sein soll. Es muss jedoch die Bedingung erfüllt sein, dass das Elternelement `position: relative` gesetzt haben muss, damit die absolute Positionierung greift.

Da meistens das direkte Kindelement das Element ist, welches als Overlay genutzt werden soll, kann man die Hilfsklasse noch ein wenig verbessern und die Definition der relativen Positionierung mit einbeziehen.

```css
.overlay-child {
  position: relative;
}

.overlay-child > * {
  position: absolute;
  content: '';
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
```
<br>

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="dypzwoK" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Covering element w/ utility class">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/dypzwoK">
  Covering element w/ utility class</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Im Codepen kann man die Hilfsklasse ganz gut togglen, indem man die Klasse einfach aus dem Element herausnimmt. 

Außerdem sieht man im Pen auch, dass es eine einfachere Variante gibt, als die `top`, `left`, `bottom` und `right` Eigenschaften zu setzen. Man kann einfach die `inset` Eigenschaft nutzen.

Zusätzlich gibt es auch eine Variante mit CSS Grid, die der Autor auch vorstellt.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="VwKzqPE" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Covering element w/ utility class AND CSS GRID">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/VwKzqPE">
  Covering element w/ utility class AND CSS GRID</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
