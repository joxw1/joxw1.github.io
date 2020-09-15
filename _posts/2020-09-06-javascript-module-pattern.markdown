---
layout: post
title: JavaScript Module Pattern
date: 2020-09-06 14:45:43 +0300
description: Kurze Vorstellung des Module Pattern in JavaScript.
img: module-pattern.jpg
fig-caption: Modul Entwurfsmuster
tags: [JavaScript, Grundlagen, Module Pattern, Closures, IIFE, Closures]
---
Das Module Pattern ist ein wichtiges Entwurfsmuster im JavaScript Umfeld. Es sorgt für eine Kapselung des Codes und einen definierten Scope für Code, der nach außen gegeben wird. Vorallem für die Entwicklung von Diensten und Tests eignet sich dieses Muster sehr gut. 

Wenn man ein Modul definiert, sind alle inne liegenden Variablen und Funktionen privat und nicht erreichbar von außen. Das ist wichtig, damit kein Code von außen den im Modul liegenden Code überschreiben kann.

```javascript
// Module Budget Controller
const module = (function() {
  const x = 5;

  const add = function(a) {
    return x + a;
  };

  return {
    publicTest: function(b) {
      console.log(add(b));
    },
  };
})(); // nach Außen ist nur die publicTest-Methode sichtbar
```

Damit das Module Pattern funktioniert, wird eine _Immediately Invoked Function Expression (IIFE)_ im Zusammenspiel mit _Closures_ verwendet (wie oben im Beispiel zu sehen). Sobal die _IIFE_ aufgerufen wird, ist der Execution Context effektiv weg. Da jedoch ein Objekt zurückgegeben wird, wird ein _Closure_ gebildet, welches Zugriff auf die inneren Variablen und Funktionen der Variable hat. 

Man sagt in diesem Fall auch, dass die _publicTest_ Funktion "public" (öffentlich) ist, da man sie von außen aus nutzen kann. Alle Funktionen die innerhalb der _IIFE_ definiert wurden, sind im Gegensatz dazu "private". 


