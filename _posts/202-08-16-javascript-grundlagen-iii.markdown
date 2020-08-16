---
layout: post
title: JavaScript Grundlagen III
date: 2020-08-08 15:24:25 +0300
description: Dritter Teil der JavaScript Grundlagen. Hier werden Funktionen und Objekte näher beleuchtet.
img: js-3.png
fig-caption: JavaScript Grundlagen III
tags: [JavaScript, Grundlagen, Objekte, Funktionen, Schleifen]
---
Im dritten Teil der Grundlagen geht es um Funktionen und Objekte. Dies ist ein fundamentaler Part und schließt damit die Grundlagensektion von [Jonas](https://www.udemy.com/course/the-complete-javascript-course) Kurs ab.

### Funktionen

Wenn man einen Teilcode öfter ausführen möchte, kann man Funktionen definieren. Diese dienen vorallem dazu, eines der wichtigsten Prinzipien beim Programmieren auszufüllen: **D**ont **R**epeat **Y**ourself (DRY).

```javascript
function calculateAge(birthYear) {
    return 2018 - birthYear;
}
```

##### Funktionsanweisungen & Funktionsausdrücke

Funktionsausdrücke sind eine andere Methode Funktionen zu schreiben.  Man weist damit eine Funktion einer Variable zu.

```javascript
// Function declaration
function whatDoYouDo(job, firstName) {}
// Function expression
var whatDoYouDo = function (job, firstName) {}
```

Funktionsausdrücke liefern immer einen Wert zurück. 

### Arrays

Arrays sind Sammlungen an Werten oder Objekten. Sie werden folgenderweise deklariert.

```javascript
var names = ['John', 'Mark', 'Jane'];
var years = new Array(1990, 1969, 1948);
//To access an element
console.log(names[0]);
//Specific member variables
console.log(names.length);
```

Über den Index kann man einen Wert im Array ändern.

```javascript
names[1] = 'Ben';
names[names.length] = 'Mary';
```

Ein besonderes Merkmal, im Vergleich zu anderen Sprachen, ist, dass ein Array unterschiedliche Datentypen halten kann. So kann zum Beispiel ein "Objekt" gebildet werden.

```javascript
var john = ['John', 'Smith', 1990, 'teacher', false]
```

Für Arrays gibt es bestimmte Methoden, die ein Array standardmäßig mit sich bringt.

```javascript
john.push('blue'); // Adds element to end of array
john.unshift('Mr.'); // Adds element to start of array
john.pop(); // Removes element from end of array
john.shift(); // Removes element from start of array
john.indexOf(1990); // Which position has 1990 in array or -1
```

### Objekte

Objekte sind ein sehr wichtiger Teil in der JavaScript Sprache, obwohl man meinen könnte, JavaScript sei nicht objektorientiert. Objekte sind, einfach gesagt, Schlüssel-Werte-Paare. 

```javascript
var john = {
    firstName: 'John',
    lastName: 'Smith',
    birthYear: 1990,
    family: ['Jane', 'Mark', 'Bob'],
    job: 'teacher'
    isMarried: false
};
```

***Merke:*** Die geschweiften Klammer nennt man"Object Literals".

Um auf ein Element aus einem Objekt zuzugreifen, kann zum einen die Punkt-Notation verwenden, oder die Klammern-Notation.

```javascript
console.log(john.firstName); // Dot-Notation
console.log(john['lastName']); // Brackets-Notation
```

Ein anderer Weg Objekte zu definieren ist, ähnlich zu den Arrays, das Objekt explizit zu instanziieren.

```javascript
var jane = new Object();
jane.name = 'Jane';
jane.birthYear = 1969;
jane['lastName'] = 'Smith';
```

##### Objektmethoden

Man kann einem Objekt auch Methoden zuweisen. Dies erfolgt folgendermaßen.

```javascript
var john = {
	firstName: 'John',
    birthYear: 1990,
    calcAge: function(birthYear) {
        return 2018 - birthYear;
    }
};
console.log(john.calcAge(1990));
```

Um innerhalb der Methoden bestimmte Werte aus einem Objekt zu referenzieren kann das Schlüsselwort "this" verwendet werden.

```javascript
var john = {
	firstName: 'John',
    birthYear: 1990,
    calcAge: function() {
        return 2018 - this.birthYear;
    }
};
console.log(john.calcAge());
```

### Schleifen

Schleifen sind ein essentieller Bestandteil einer jeden Programmiersprache und bieten die Möglichkeit, Berechnungen zu skalieren und Code zu reduzieren.

###### For-Schleife

Um eine for-Schleifen zu definieren muss man folgenderweise vorgehen.

```javascript
var john = ['John', 'Smith', 1990];
for (var i = 0; i < 3 i++) {
	console.log(john[i])
}
```

###### While-Schleife

Eine andere Variante von Schleifen ist die while-Schleife.

```javascript
var i = 0;
while(i < john.length) {
	console.log(john[i]);
	i++;
}
```