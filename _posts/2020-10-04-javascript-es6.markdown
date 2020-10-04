---
layout: post
title: JavaScript ES6
date: 2020-10-04 15:40:23 +0300
description: Notizen zu den neuen Funktionen in ES6.
img: es6.jpg
fig-caption: Neue Funktionen in ES6
tags: [JavaScript, Grundlagen, ES6, ES2015, Blöcke, let, const, String Literals]
---
Mit ES6 (bzw. ES2015) sind einige große Änderungen zur JavaScript Sprache dazugekommen. In diesen Notizen werde ich die wichtigen Änderungen festhalten und kurz mit der Syntax aus ES5 vergleichen. Der Inhalte kommt wieder aus dem Kurs von [Jonas](https://www.udemy.com/course/the-complete-javascript-course/learn/lecture/6034300#overview).

### var - let - const

Bei den Variablendeklarationen gibt es zwei neue Schlüsselwörter: **let** und **const**. Mit dem Schlüsselwort **let** können flüchtige Variablen deklariert werden. Das bedeutet, die Werte der Variablen können wieder neu gesetzt werden (wie bei *var*). Das Schlüsselwort **const** hingegen wird dazu genutzt Konstanten zu definieren. Die Werte von Konstanten können nicht verändert werden.

```javascript
// ES5
var nameES5 = 'Jane Smith';
var ageES5 = 23;
nameES5 = 'Jane Miller';
console.log(nameES5);

// ES6
const nameES6 = 'Jane Smith';
let ageES6 = 23;
nameES6 = 'Jane Miller';
console.log(nameES6); // TypeError!
```

Ein großer Unterschied zwischen *var*, *let* und *const* ist, dass sie einen unterschiedlichen Scope (Namensbereich) besitzen. Die Schlüsselworte *let* und *const* sind **block-scoped**  - Das bedeutet, dass sie innerhalb eines Blockes gültig sind. Das Schlüsselwort *var* hingegen ist **function-scoped**, was bedeutet, dass innerhalb einer Funktion die Variable nur einmal deklariert werden kann. Im unten stehenden Beispiel wird dieser Unterschied deutlich. Im ES6-Beispiel kann die Variable außerhalb des If-Blocks nicht genutzt werden, da sie eine *let*-Variable ist und daher nicht außerhalb des Blocks verfügbar ist.

```javascript
// ES5
function driversLicenceES5(passedTest) {
  if (passedTest) {
    var firstName = 'John';
    var yearOfBirth = 1995;
  }
  console.log(firstName + ', born in '+yearOfBirth+
    ', is now officially allowed to drive a car.');
}

// ES6
function driversLicenceES6(passedTest) {
  if (passedTest) {
    let firstName = 'John';
    const yearOfBirth = 1995;
  }
  // Is not defined error!
  console.log(firstName + ', born in '+yearOfBirth+
    ', is now officially allowed to drive a car.');
}
```

### Blöcke und IIFE (Immediately Invoked Function Expressions)

In ES5 musste man noch, um Datensicherheit zu erreichen, die *Immediately Invoked Function Expressions* verwenden. Da in ES6 jedoch die neuen Schlüsselworte dazugekommen sind, kann nun die neue Methodik verwendet werden um diese Datensicherheit zu erreichen.

```javascript
// ES5 (IIFE)

(function() {
  var c = 3;
})();

console.log(c); // Not defined!

// ES6 (Block)
{
  const a = 1;
  let b = 2;
  var d = 3;
}

console.log(d); // = 3
console.log(b); // Not defined!
console.log(a); // Not defined!
```

Wie man im Beispiel sieht, werden nicht mehr die *IIFE* benötigt um Datenstrukturen privat zu machen. Es genügt eine geschweifte Klammer, um einen Block zu definieren.

### Strings

Ein neues Feature in ES6 sind die *String-Literals*. Damit lassen sich Variablen in Strings leichter einbauen, wie im unteren Beispiel zu sehen.

```javascript
let firstName = 'John';
let lastName = 'Smith';
const yearOfBirth = 1995;
function calcAge(year) {
  return parseInt(new Date().getFullYear()) - year;
}

// ES5
console.log('This is '+firstName+' '+lastName+' - he is '+calcAge(yearOfBirth)+' years old');

// ES6
console.log(`This is ${firstName} ${lastName} - he is ${calcAge(yearOfBirth)} years old`);
```


