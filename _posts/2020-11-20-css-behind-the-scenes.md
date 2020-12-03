---
layout: post
title: CSS hinter den Kulissen
date: 2020-11-20 22:04:21 +0300
description: Notizen über CSS hinter den Kulissen und wie CSS geparsed wird.
img: cascading.jpg
fig-caption: Cascading
tags: [CSS, Grundlagen, Behind the Scenes, Responsive, Vererbung]
---
Im Kurs von [Jonas](https://www.udemy.com/course/advanced-css-and-sass/) gibt es einen schönen Einblick in die theoretischen Teile von CSS (Cascading Style Sheets). Diese Lektionen waren besonders interessant und werden daher von mir nochmal extra in einem Post festgehalten.

Grundsätzlich ist die Struktur des Ladens einer Website mit HTML und CSS folgendermaßen:

1. HTML wird geladen
2. HTML wird geparsed
   1. Document Object Model wird generiert
3. CSS wird geladen
4. CSS wird geparsed
   1. Auflösen widersprüchlicher CSS-Deklarationen (cascade)
   2. Finale CSS-Werte verarbeiten
   3. Das CSS Object Model (CSSOM) wird generiert
5. DOM und CSSOM bilden den "Render tree"
6. Website wird gerendert

In diesen Notizen wird nun näher auf die CSS spezifischen Teile eingegangen und das theoretische Modell dahinter beleuchtet.

## Wie wird CSS geparsed?

CSS besteht immer aus einem Selektor und einem Deklarationsblock. Jede Deklaration im Deklarationsblock besteht widerum aus einer Eigenschaft (Property) und einem Wert (Declared value).

```css
.selector {
	color: blue;
	text-align: center;
}
```

Beim Verarbeiten von CSS werden, wie bereits oben erwähnt,  zuerst widersprüchliche CSS-Deklarationen aufgelöst. Das kann zum Beispiel auftreten, wenn eine Regel (Deklaration) auf mehrere Elemente angewendet wird. 

Nach welcher Entscheidungsfolge löst der CSS-Parser die Widersprüche auf?

1. Bedeutung (Importance)
2. Besonderheit / Spezifität (Specificity)
3. Reihenfolge (Source order)

#### Bedeutung

Die erste Entscheidungsgröße ist die Bedeutung. Also wie wichtig ist eine Deklaration. Dabei gibt es auch eine Prioritätsreihenfolge.

1. `!important` Deklarationen vom Nutzer
2. `!important` Deklarationen vom Autor
3. Autor Deklarationen
4. Nutzer Deklarationen
5. Browser Deklarationen

#### Besonderheit / Spezifität

Wenn Elemente die gleiche Bedeutung bzw. Wichtigkeit besitzen, wird nach der Spezifität entschieden. In der Prioritätsreihenfolge wird klar, worum es dabei geht.

1. inline Styles
2. ID's
3. Klassen, Pseudo-Klassen und Attribute
4. Elemente, Pseudo-Elemente

```css
/*Specificity = (inline-style, ids, classes, elements)*/
.button {
	color: white; 
} /* specificity = (0, 0, 1, 0) */

nav#nav div.pull-right .button {
	color: red;
} /* specificity = (0, 1, 2, 2) */

a {
    color: blue;
} /*specificity = (0, 0, 0, 1) */

#nav a.button:hover {
    color: green;
} /*specificity = (0, 1, 2, 1) */

/* Winner is red (0, 1, 2, 2) ! */
```

#### Reihenfolge

Zum Schluss wird dann nach der Reihenfolge der Deklarationen entschieden. *Die Letzte gewinnt!*

## Wie werden CSS-Werte verarbeitet?

Die Werte, die in den CSS-Deklarationen bestimmt werden, werden in sechs Schritten verarbeitet, bis man vom deklarierten zum finalen Wert kommt.

#### 1. Deklarierter Wert

Der erste Wert, der ermittelt wird, ist der deklarierte Wert. Dies können auch mehrere Werte in sich widersprüchlichen Deklarationsblöcken sein (siehe Spezifität).

```css
p {
	width: 140px;
}

.text {
    width: 66%;
}
```

#### 2. Kaskadierter Wert

Als nächstes wird der kaskadierte Wert ermittelt. Dies ist im letzten Abschnitt beschrieben.

#### 3. Spezifizierter Wert

Falls kein kaskadierter Wert vorhanden ist, wird ein spezifizierter Wert genutzt. Dies ist wie ein Default-Wert und kann über Vererbung ermittelt werden.

#### 4. Berechneter Wert

In diesem Schritt werden relative Einheiten in Pixel umgerechnet. Pixel sind die absolute Messgröße für die Darstellung von Elementen.

#### 5. Genutzter Wert

In diesem Schritt werden die letzten Berechnungen zu Messgrößen getätigt, die auf dem Layout basieren (z.B. %-Werte).

#### 6. Tatsächlicher Wert

Da Browser nur mit ganzzahligen Pixelwerten umgehen können, werden im letzten Schritt alle Gleitkommazahlen umgerechnet.

## Wie werden relative Einheiten in Pixel umgerechnet?

**%** - Bei Prozenten  wird immer die Referenzgröße des Elternelements genommen und mit dem Prozentwert multipliziert

```css
.parent {
	width: 1000px;
}
.child {
	width: 10%; /* == 100px */
}
```

**em** - Die Einheit `em` ist auch eine Font-basierte Einheit, welche jedoch auch für Längen genutzt werden kann. Für Fontgrößen ist dies lediglich der Faktor, mit dem die Größe <u>des Elternelements</u> multipliziert werden soll. Für <u>längenbasierte Einheiten</u> wird die Größe des aktuellen Elements mit dem `em`-Faktor multipliziert.

```css
.root {
	font-size: 16px;
}
.parent {
    font-size: 150%;
	padding: 2em; /*Length-based so = (2 * 24 = 48px)*/
}
.child {
    font-size: 3em; /* Font-base so = (3 * 24 = 72px)*/
}
```

**rem** - Die Einheit `rem` verhält sich ähnlich wie `em`, setzt jedoch die Referenz immer auf das Root-Element (Wurzelelement). Das bedeutet, dass egal auf welcher Ebene, die Fontgröße der Wurzel als Multiplikationsreferenz genutzt wird.

```CSS
.root {
	font-size: 16px;
}
.parent {
	font-size: 2em;
}
.child {
	font-size 10em; /* = 160px */
    padding: 10em; /* = 160px */
}
```

**vh / vw** - Diese Einheiten stehen für "viewport height (vh)" bzw. "viewport width (vw)" und stehen, wie der Name bereits aussagt, für das Verhältnis der angegebenen Größe zum Viewport des Browsers. Dabei entspricht der Wert 1vw gleich 1% des Viewports.

```CSS
.element {
	width: 90vw; /* 90% of the current viewport width*/
}
```

*Merke:* 

- Jede CSS Eigenschaft (Property) hat einen initialen Wert, der genutzt wird, wenn nichts anderes deklariert wurde und es keine Vererbung gibt
- Browser geben eine **root font-size** für jede Seite vor (meistens 16px)
- Prozentwerte und relative Einheiten werden immer in Pixel konvertiert
- Prozentwerte werden relativ zu der Schriftgröße (Fontgröße) der Elternelemente berechnet, wenn sie eine Schriftgröße beschreiben
- Prozentwerte werden relative zu der Breite der Elternelemente berechnet, wenn sie eine Länge beschreiben.
- `em` werden relativ zu der Schriftgröße der Elternelemente berechnet, wenn sie eine Schriftgröße beschreiben
- `em` werden relativ zu der eigenen Schriftgröße berechnet, wenn sie Längen beschreiben
- `rem` werden immer relativ zu der **root font-size** berechnet
- `vh` und `vw` sind einfach prozentuale Werte der Höhe und Breite des Viewport

## Vererbung

Ein wichtiges Konzept in CSS ist die Vererbung von Eigenschaften. Jede Eigenschaft hat einen initialen Wert, es sei denn es gibt einen vererbten Wert. Daher ist es wichtig zu verstehen, wie Werte vererbt werden können.

1. Gibt es einen kaskadierten Wert?
   1. Wenn ja, dann wird der spezifizierte Wert auf den kaskadierten Wert gesetzt
   2. Wenn nein, dann wird überprüft ob die Eigenschaft vererbt wird (spezifisch für jede Property)
      1. Wenn ja, dann wird der spezifizierte Wert auf den berechneten Wert des Elternelements (vererbt) gesetzt
      2. Wenn nein, wird der spezifizierte Wert auf den initialen Wert gesetzt (spezifisch für jede Property)

```css
.parent {
	font-size: 20px;
	line-height: 150%;
}
.child {
	font-size: 25px;
	/*line-height is inherited as stated in specs*/
}
```

*Merke:*

- Vererbung sorgt dafür dass **manche** Werte für bestimmte Properties von den Eltern zu Kindselementen übergeben werden
- Als Daumenregel lässt sich sagen: Properties, die die Schrift betreffen werden in der Regel vererbt
- Der Wert der vererbt wird, ist der **berechnete Wert** und **nicht der deklarierte Wert**
- Vererbung funktioniert nur, wenn für eine Property nicht schon ein Wert deklariert wurde
- Das `inherit` Schlüsselwort forciert Vererbung für ein Property
- Das `initial` Schlüsselwort setzt den Wert für eine Property auf den initialen Wert zurück

### Visuelles Formatierungsmodell

Das visuelle Formatierungsmodell bestimmt wie die Webseite letztendlich gerendert wird und ist der letzte Arbeitsschritt vorm Anzeigen der Webseite.

*Definition: Algorithmus, der Boxen berechnet und das Layout dieser Boxen für jedes Element im Renderbaum bestimmt, um das endgültige Layout der Seite zu bestimmen.* 

Der Algorithmus bezieht dabei unterschiedliche Elemente ein:

- **Dimensionen der Boxen**: Das Box-Modell
- **Typ der jeweiligen Box**: inline, block und inline-block
- **Positionsschema**: floats und Positionierung
- **Stapelung von Kontexten**
- Viewport-Größe, Dimensionen von Bilder, etc.

#### Das Box-Modell

Das Box-Modell ist eins der wichtigsten Bestandteile von CSS. Um eine Webseite zu gestalten. Es entscheidet darüber, wie ein Element dargestellt wird und welche Größe es hat.

![](https://media.prod.mdn.mozit.cloud/attachments/2015/07/20/11243/c45d8c4d298932a08344a60a575c4a63/%3DCSS_Boxmodell_changed.png)

Nach dem Box-Modell kann jedes Element auf einer Webseite als eine rechteckige Box gesehen werden. Ein Element kann dabei *padding*, *margin*, *width*, *height* und ein *border* besitzen. Diese sind alle optional.

- Inhalt: Der Inhalt einer Box können jegliche Inhalte wie z.B. Text oder Bilder sein
- **Padding**: Dies ist ein transparenter Bereich um den Inhalt, jedoch innerhalb der Box
- **Border**: Ist der Rand der Box.
- **Margin**: Dies ist der Abstand zwischen Boxen, außerhalb der Box für den Inhalt.
- **Fill area**: Das ist der gesamte innere Bereich der Box, welche mit Farbe oder einem Bild ausgefüllt werden kann. 

Die Höhe und Breite einer Box werden, unter Berücksichtigung der gesamten Elemente der Box, folgendermaßen berechnet:

- **Breite:** `border` rechts + `padding` rechts + `width` des Elements + `padding` links + `border` links
- **Höhe:** `border` oben+ `padding` oben+ `height` des Elements + `padding` unten+ `border` unten

#### Box-Typen

- **Block-Level Boxen**: Elemente werden visuell als Blöcke formatiert und besitzen 100% der Breite des Elternelements. Sie werden vertikal, nacheinander dargestellt. 

  ```css
  body {
    display: block;
  /*display: flex; 
    display: list-item;
    display: table;*/
  }
  ```

- **Inline-Block Boxen**: Sie sind ein Hybrid aus `inline` und `block` Boxen. Sie nehmen nur den Platz ein, den sie benötigen und produzieren auch keine Zeilenumbrüche. Man kann jedoch das Box-Modell auf ihnen anwenden.

  ```css
  body {
    display: inline-block;
  }
  ```

- **Inline Boxen**: Elemente nehmen nur so viel Platz ein, wie sie benötigen und produzieren keine Zeilenumbrüche vor- und nacheinander. Sie stellen somit das Gegenteil der `block`-Boxen dar. Man sollte dabei beachten, dass `inline`-Boxen die `height` und `width` Eigenschaften nicht besitzen. Außerdem können `padding` und `margin` nur horizontal (links und rechts) definiert werden.

  ```css
  body {
    display: inline;
  }
  ```

### Normal Flow, Floats und absolute Positionierung

Der "Normal Flow" ist das standardmäßige Positionsschema und bedeutet, dass die Elemente in der Reihenfolge der Definition im Code angeordnet sind. Sie werden somit nicht absolut positioniert (nach Koordinaten) sondern stehen in Relation zu den ihnen umgebenden Elementen. 

```css
.element {
  position: relative;
}
```

Nutzt man für ein Element die `float`-Positionierung, bedeutet das, dass das Element aus dem "Normal Flow" ausgenommen wird und nach links oder rechts  verschoben wird bis es den Rand der umgebenden Box berührt. Texte und `inline`-Blöcke werden dabei um das `float`-Element "gewickelt". 

```css
.element {
  float: left;
/*float: right;*/
}
```

Eine andere Positionierungsart ist die absolute Positionierung. Auch bei dieser Positionierungsart wird das Element aus dem "Normal Flow" herausgenommen. Im Gegensatz zum `float` beeinflusst das aber nicht die umgebenden Elemente.  Die Position wird dabei in Relation zum dem Element umgebenden Container angegeben. Dazu kann man die Eigenschaften `top`, `bottom`, `left` und `right` verwenden.

```css
.element {
  position: absolute;
/*position: fixed;*/
}
```

### Stacking Contexts

Eine wichtige Eigenschaft in CSS ist die Überlappung von verschiedenen Elementen ("Stacking Contexts"). Man kann sich dies wie Ebenen in Photoshop vorstellen, wo man definiert, welche Ebene oben bzw. unten ist. Man kann die Anordnung der Ebenen z.B. mit dem z-Index beeinflussen. Dabei werden Elemente mit hohem z-Index weiter oben (mehr im Vordergrund) dargestellt. 

```css
.element {
  z-index: 3;
  position: relative;
}
```

