---
layout: post
title: Document Object Model (DOM)
date: 2020-08-19 21:37:15 +0300
description: Meine Notizen zum Document Object Model in Bezug auf Vanilla JavaScript.
img: dom.jpg
fig-caption: Document Object Model
tags: [JavaScript, Grundlagen, DOM, Manipulation, Events, Event Listener]
---
# Document Object Model (DOM)

Im nächsten Schritt zu den Java Grundlagen (aus [diesem Kurs](https://www.udemy.com/course/the-complete-javascript-course/)) steht das _Document Object Model_. Dies spiegelt die strukturierte Repräsentation einer HTML-Seite mit dessen Tags und Verschachtelungen dar und ist die Schnittstelle zwischen Webseite und JavaScript.

Um auf das _Document Object_ zuzugreifen, kann man das Schlüsselwort "document" verwenden.

```javascript
document.querySelector('#someRandomId');
```

#### DOM-Manipulation

Um Elemente aus dem DOM zu manipulieren, kann der o.g. Query-Selector genutzt werden. Mit ihm kann man, wie in CSS, Elemente aus dem DOM auswählen.

Über die Member-Variable "textContent" kann man dann z.B. den textuellen Wert setzen.

```javascript
var score0 = document.querySelector(`#current-0`);
score0.textContent = '5';
```

__Wichtig:__ "textContent" setzt nur einen reinen Text-Wert. Möchte man HTML-Code in das gewünschte Element setzen, muss die Member-Variable "innerHTML" verwendet werden.

```javascript
document.querySelector('#current-0').innerHTML = '<b>5</b>';
```

##### Element über ID

Ein anderer Weg, um ein Element auszuwählen, kann die Methode "getElementById(...)" sein. Dazu muss im HTML-Tag, welches man auswählen möchte, eine ID definiert sein.

```javascript
document.getElementById('score-0').textContent = '0';
```

##### CSS-Style ändern

Um den Style eines Elements zu ändern, kann man die Member-Variable "style" verwenden.

```javascript
document.querySelector('.dice').style.display = 'none';
```

##### Klassen hinzufügen

Man kann einem HTML-Element Klassen hinzufügen (oder entfernen). Außerdem kann man eine Klasse auch togglen (also hinzufügen oder entfernen, je nachdem ob sie schon vorhanden ist).

```javascript
document.querySelector(`.player`).classList.add('active');
document.querySelector(`.player`).classList.remove('active');
document.querySelector(`.player`).classList.toggle('active');
```

#### Event-Handling

**Events** sind Benachrichtigungen, die von der Webseite an den "Code" gesendet werden. Diese können z.B. durch einen Buttonklick, einen Tastendruck oder das Verstellen der Fenstergröße ausgelöst werden. Events können durch **Event Listener** abgefangen werden. Dies sind Funktionen, die Aktionen basierend auf dem ausgelösten Event tätigen.

**Wichtig:** Ein Event kann erst behandelt werden wenn der [Execution Stack](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/) leer ist!

Neben dem o.g. Execution Stack gibt es eine Message Queue, in welcher die wartenden Event's abgearbeitet werden (sobald der Stack leer ist). Wenn ein Event aus der Message Queue abgearbeitet wird, wird dies zum neuen Execution Context.

##### Event-Listener hinzufügen

Um einen Event-Listener zu einem Element hinzufügen, kann man einfach das gewünschte Element über den Query-Selector auswählen und einen Event-Listener hinzufügen.

```javascript
document.querySelector('.btn-roll').addEventListener('click', event => {})
```

Eine Auflistung der Event-Typen (im Beispiel "click") findet man [hier](https://developer.mozilla.org/de/docs/Web/Events).

