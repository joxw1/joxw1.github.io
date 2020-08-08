---
layout: post
title: JavaScript Grundlagen I
date: 2020-08-04 20:27:20 +0300
description: 
img: js-1.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [JavaScript, Grundlagen]
---
Dies ist der erste Teil meiner Notizen über JavaScript Grundlagen, basierend auf dem Kurs von [Jonas](https://www.udemy.com/course/the-complete-javascript-course/). Diese Reihe fange ich an, um noch einmal von Grund auf meine JavaScript Kenntnise aufzufrischen und ein gutes Fundament für das Erlernen von Frameworks wie Angular oder React zu haben.

## Sektion 1: JavaScript Basics

JavaScript ist eine plattformunabhängige, objekt-orientierte Programmiersprache, die zusammen mit HTML und CSS das "Rückgrat" der Web-Entwicklung darstellt.

### Primitive Datentypen

Da JavaScript grundsätzlich objektorientiert arbeitet, kann man Werte zu Variablen zuweisen.

```javascript
var firstName = 'John'; // "John" würde auch gehen
```

Die Arten an Zuweisungen, die man machen kann sind:

| Typ       | Beschreibung                                     |
| --------- | ------------------------------------------------ |
| Number    | Zahlen (Sind **immer** Gleitkommazahlen!)        |
| String    | Text (Zeichenketten und Zeichen)                 |
| Boolean   | Wahrheitswert (Logischer Typ `true` und `false`) |
| Undefined | Datentyp ohne Wert (Leere Hülle)                 |
| Null      | Nicht existent                                   |

**Wichtig!** JavaScript besitzt eine dynamische Typisierung - Das bedeutet, dass der Datentyp automatisch zu einer Variable zugewiesen wird. Man könnte daher auch annehmen, dass JavaScript eine nicht-typisierte Sprache ist.

#### Beispiele

```javascript
var lastName = 'Smith'; // String
var age = 28; // Number
var fullAge = true; // Boolean
```
###  Mutation von Variablen und Typ-Zwang

#### Typ-Zwang

JavaScript kann Variablen mit einem zugewiesenen Typ automatisch umwandeln, wenn dies benötigt wird. Dies nennt man "Type coercion" (Typ-Zwang). Das kommt z.B. vor, wenn Variablenwerte über die Konsolenausgabe zu Strings umgewandelt werden müssen.

#### Mutation von Variablen

Die Mutation von Variablen bedeutet, den Typen einer Variable zu verändern. Ist z.B. eine Variable mit einem String "belegt", kann jederzeit die Variable mit einer Nummer neu belegt werden.

### Basisoperatoren

Grundsätzlich können alle mathematischen Grundoperatoren auch in JavaScript verwendet werden. Zusätzlich bietet JavaScript jedoch noch weitere Operatoren, wie z.B. logische Operatoren.

| Typ  | Beschreibung   |
| :--: | -------------- |
|  +   | Addition       |
|  -   | Subtraktion    |
|  *   | Multiplikation |
|  /   | Division       |
|  >   | "größer als"   |
|  <   | "kleiner als"  |
