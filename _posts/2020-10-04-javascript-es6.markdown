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

Weiterhin gibt es in ES6 einige neue Methoden für Strings. Nachfolgend sind ein paar aufgelistet.

```javascript
let firstName = 'John'
let lastName = 'Smith'
const n = `${firstName} ${lastName}`;
n.startsWith('J'); // true
n.startsWith('j'); // false
n.endsWith('th'); // true
n.includes(' '); // true
firstName.repeat(2); // JohnJohn
```

### Arrow-Funktionen

Einige wichtige neue Änderung in ES6 sind die Arrow-Funktionen. Sie dienen dazu, Callback-Funktionen kürzer auszudrücken.

```javascript
const years = [1990, 1995, 1965, 1982, 1937];
// ES5
var ages5 = years.map(function(elem) {
    return 2020 - elem;
});
// ES6
let ages6 = years.map(elem => 2020 - elem);
ages6 = years.map((elem, index) => `Age element ${index + 1}: ${2020 - elem}`);
```

Arrow-Funktion haben kein eigenes _this_-Schlüsselwort, stattdessen nutzen sie das _this_ aus den Funktionen, die sie aufrufen. Man sagt dazu auch _lexikalisches this_.

```javascript
// ES5
var box5 = {
  color: 'green',
  position: 1,
  clickMe: function() {
    var self = this; // The this in 'clickMe' will point to window
    document.querySelector('.green').addEventListener('click', function() {
      var str = 'This is box number '+self.position+ ' and it is '+self.color;
      alert(str);
    });
  },
};

box5.clickMe();

// ES6
var box6 = {
  color: 'green',
  position: 1,
  clickMe: function() {
    document.querySelector('.green').addEventListener('click', () => {
      var str = 'This is box number '+this.position+ ' and it is '+this.color;
      alert(str);
    });
  },
};

box6.clickMe();
```

### Destructuring

In ES6 wurde eine Methode eingeführt, Datenstrukturen zu dekonstruieren. Das bedeutet, dass bereits initialisiert Datenstrukturen in ihre Einzelteile aufgeteilt werden können. So kann man z.B. Variablen in ihre Bestandteile zerlegen um die Bestandteile weiter zu verarbeiten.

```javascript
// ES5
var john = ['John', 24];
var name5 = john[0];
var age5 = john[1];

// ES6
const [name6, age6] = ['John', 24];

const obj = {
  firstName: 'John',
  lastName: 'Smith',
};

const {firstName, lastName} = obj;
const {fistName: a, lastName: b} = obj;
```

### Arrays

Auch Arrays wurden in ES6 mit neuen Methoden ausgestattet. Nachfolgend wird ein nützliches Beispiel betrachtet.

```javascript
const boxes = document.querySelectorAll('.box');

// ES5
var boxesArr5 = Array.prototype.slice.call(boxes);
boxesArr5.forEach(function(cur) {
  cur.style.backgroundColor = 'dodgerblue';
});

// ES6
const boxesArr6 = Array.from(boxes);
boxesArr6.forEach(cur => cur.style.backgroundColor = 'dodgerblue');
```

