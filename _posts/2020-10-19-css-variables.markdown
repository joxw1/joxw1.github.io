---
layout: post
title: CSS Variablen
date: 2020-10-19 15:07:12 +0300
description: Notizen über CSS Variablen
img: css.jpg
fig-caption: CSS Variablen
tags: [CSS, Grundlagen, Variablen, var]
---
Ich habe vor kurzem einen interessanten Artikel über CSS Variablen gelesen und würde die Informationen aus diesem Artikel gerne hier festhalten. Für das Original bitte [hier](https://ishadeed.com/article/css-vars-101/) klicken.

CSS Variablen dienen der Komplexitätsreduktion und des Herunterschraubens von Redundanz. Sie ermöglichen eine Wiederverwertung von bestimmten Werten, die häufiger in einem Stylesheet auftauchen. Ahmad hat in seinem Artikel viele interessante Beispiele, die ich hier übernehme.

Als Beispiel:

````css
.section {
  border: 2px solid #235ad1;
}

.section-title {
  color: #235ad1;
}

.section-title::before {
  content: "";
  display: inline-block;
  width: 20px;
  height: 20px;
  background-color: #235ad1;
}
````

Man sieht, dass die Farbe `#235ad1` insgesamt dreimal auftaucht. Das könnte man vereinfachen, in dem man eine Variable nutzt. Sie wird in einem Block mit einem Doppelstrich deklariert, wie man im nachfolgenden Beispiel sieht.

```css
:root {
  --color-primary: #235ad1;
}

.section {
  border: 2px solid var(--color-primary);
}

.section-title {
  color: var(--color-primary);
}

.section-title::before {
  /* Other styles */
  background-color: var(--color-primary);
}
```

So sieht es schon deutlich schöner aus und man hat das Prinzip *Don't Repeat Yourself* beachtet.

### Benennung

Wichtig ist, wie bei anderen Programmiersprachen, die richtige Benennung der Variablen. Es sind alphanumerische Zeichen, Unterstriche und Bindestreiche erlaubt. Leerzeichen und Sonderzeichen dürfen zuhause bleiben 🚫.

```css
/* Valid names */
:root {
	--primary-color: #222;
	--_primary-color: #222;
	--12-primary-color: #222;
	--primay-color-12: #222;
}

/* Invalid names */
:root {
	--primary color: #222; /* Spacings are not allowed */
	--primary$%#%$#
}
```

### Geltungsbereiche

Bei CSS Variablen gibt es auch unterschiedliche Geltungsbereiche. Eine Variable kann in einem Deklarationsblock überschrieben werden. Im nachfolgenden Beispiel sieht man, dass die `--primary-color` Variable in der `.section-title` Klasse überschrieben wird. Der Wert gilt dann nur in dem Block, in dem es neu definiert wurde.

```css
:root {
  --primary-color: #235ad1;
}

.section-title {
  --primary-color: d12374;
  color: var(--primary-color);
}
```

### Fallback

Bisher wurde die `var`-Funktion immer nur mit einem Parameter beachtet. Man kann jedoch noch einen zweiten, sehr nützlichen Parameter übergeben - den **Fallback**-Wert. Dieser Wert wird genutzt, wenn der Wert der Variable nicht genutzt bzw. ermittelt werden kann. Man kann dies auch mit einem erneuten `var`-Aufruf verketten.

```css
/* Normal fallback value */
.section-title {
  color: var(--primary-color, #222);
}
/* Chained call */
.section-title {
  color: var(--primary-color, var(--black, #222));
}
```

### Use-Cases

Ahmad hat in seinem Artikel bereits coole Use-Cases mitgeliefert. Ich habe diese  Sachen in Codepen's nachimplementiert um ein Gefühl dafür zu bekommen, wie es zu nutzen ist.

#### Größe von Elementen

Das erste Beispiel ist, die Größe von Elementen dynamisch anhand ihrer gesetzten Klasse zu verändern. Man nutzt dabei gezielt den Geltungsbereich (**Scope**) der Variablen. 

Im nachfolgenden Beispiel sieht man, dass in der Hauptklasse `.button` ein Padding definiert ist, welches von der Variable `--unit` abhängt. Initial ist die Einheit `1rem` groß. Möchte man nun einen kleineren/größeren Typ des Buttons erstellen, reicht es, die `--unit`-Variable zu überschreiben.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="WNxxJEz" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Variables - Size of elements">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/WNxxJEz">
  CSS Variables - Size of elements</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

#### Farbe von Elementen

Ein anderes schönes Beispiel ist die dynamische Anpassung von Farben über CSS Variablen.

Im Beispiel ist die Hintergrundfarbe über die `hsl`-Funktion gesetzt. Der Wert der Funktion besteht aus Farbton, Sättigung und Helligkeit. Sind diese Werte, wie im Beispiel zu sehen, über Variablen gesetzt, kann man einzelne Werte dynamisch anpassen. Hier wird die Helligkeit heruntergesetzt, sobald man über den Button hovert. Das ist ziemlich cool, da man Farbton und Sättigung somit behält und einen schönen Effekt erzielt.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="YzWWvea" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Variables - Color of element">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/YzWWvea">
  CSS Variables - Color of element</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script> 

#### Proportionale Größenänderung

Dieses Beispiel ist auch sehr cool. Man kann sich die Wiederverwendung von Variablen zu nutze machen, um Breite und Höhe eines Elements immer proportional zueinander zu halten. 

Im nachfolgenden Pen habe ich versucht, dies dynamisch zu bauen.

<p class="codepen" data-height="441" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="BazzPdo" style="height: 441px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Variables - Proportional Resize">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/BazzPdo">
  CSS Variables - Proportional Resize</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

#### Light-Mode und Dark-Mode

Ein anderer interessanter Use-Case ist das Umschalten von Light-Mode und Dark-Mode. Man muss dazu lediglich ein Farbschema für die Variablen festsetzen und diese Variablen durchgängig benutzen und kann damit leicht zwischen den beiden Modi durchtogglen. Ich habe mal versucht, ein Minimalbeispiel in einen Pen zu packen.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="bGeejxB" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Variables: Switch mode">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/bGeejxB">
  CSS Variables: Switch mode</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

#### Animationen

Ein weiteres Beispiel für das Nutzen von Variablen ist für Animationen. Man kann die Variablen in den Keyframes nutzen und die Animation so, je nach aktivierter Klasse, verändern. Damit lassen sich interessante Effekte erzielen. Ein kleines Beispiel ist nachfolgend gezeigt.

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="css,result" data-user="joxwi" data-slug-hash="gOMMZqX" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Variables - Animations">
  <span>See the Pen <a href="https://codepen.io/joxwi/pen/gOMMZqX">
  CSS Variables - Animations</a> by Joshua Wiegmann (<a href="https://codepen.io/joxwi">@joxwi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Zusammenfassung

Das waren meine Notizen zum Post von Ahmad. Dabei habe ich längst nicht alles aufgeschrieben, was an Inhalt im Original-Post drin war - daher lohnt sich der [Blick in den Post nochmal sehr](https://ishadeed.com/article/css-vars-101/), um alles detailliert nachzulesen.

Für die Codepens in diesen Artikel [habe ich eine Collection angelegt](https://codepen.io/collection/DEzrEJ).