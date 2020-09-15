---
layout: post
title: JavaScript Objekte und Funktionen
date: 2020-08-19 21:37:15 +0300
description: Meine Notizen zu Objekte und Funktionen in Vanilla JavaScript.
img: objects.jpg
fig-caption: Objekte und Funktionen
tags: [JavaScript, Grundlagen, Objekte, Funktionen, IIFE, Closures, Konstruktor]
---
In JavaScript gibt es zwei unterschiedliche Art von Datentypen: Grundelemente (_Primitives_) und Objekte (_Objects_). Zahlen (_Numbers_), Zeichenketten (_Strings_), _Booleans_, _Undefined_ und _Null_ sind Grundelemente. Alles andere sind Objekte (z.B. _Arrays_ oder _Funktionen_).

#### Objektorientierte Programmierung

In der objektorientierten Programmierung stehen, wie der Name vermuten lässt, die Objekte im Fokus. Objekte können Daten über Methoden und Eigenschaften miteinander austauschen und verarbeiten. Um nicht jede Struktur zu einem individuellen Objekt implementieren zu müssen, gibt es in JavaScript die Möglichkeit "Objekttypen" zu deklarieren. Dies ist ein Art Blaupause für alle Strukturen die auf den Objekttyp passen. In anderen Programmiersprachen spricht man von **Klassen**, währen in JavaScript von **Konstrukturen** oder **Prototypen** gesprochen wird.

> _*Notiz:*_ Ich würde von Prototypen sprechen, da der Begriff Konstruktor aufgrund der Vorbelegung aus Compiler-Sprachen vorbelastet ist.

##### Vererbung

Ein weiter wichtiger Aspekt ist die Vererbung von __Prototypen__. Bei der Vererbung werden durch eine Spezialisierung eines Objektes bestimmte Eigenschaften hinzugefügt, die die Basisklasse, von der das spezialisierte Objekt abstammt, nicht besitzt. 

> *Notiz:* Wenn man zum Beispiel ein Objekt "Tier" hat, wäre eine Spezialisierung das Objekt "Hund" oder das Objekt "Fisch". Ein "Hund" könnte Eigenschaften haben wie "Anzahl an Beinen". Diese Eigenschaft würde sich der "Hund" nicht mit dem "Fisch" teilen, trotzdem stammen sie beide vom "Tier" ab.

##### Prototypen

Um eine Vererbung zu erreichen, müssen Eigenschaften als Prototypen-Eigenschaften deklariert sein. Alles was in den Prototypen-Eigenschaften definiert wird kann von den Objekt-Instanzen genutzt werden.

> __Merke:__
>
> 1. Jedes JavaScript Objekt hat eine Prototyp-Eigenschaft, was Vererbung möglich macht.
> 2. In der Prototyp-Eigenschaft eines Objekts legt man Methoden und Eigenschaften fest, die andere Objekte erben sollen.
> 3. Die Prototyp-Eigenschaft des Konstruktor ist nicht der Prototyp des Konstruktor selbst, sondern der Prototyp aller Instanzen, die durch ihn entstanden sind.
> 4. Wenn eine Methode aufgerufen wird, wird zuerst im Objekt nach der Methode gesucht und wenn es nicht gefunden werden kann, wird im Prototyp des Objektes nach der Methode gesucht. Das kaskadiert nach "oben" bis die Methode gefunden wird. Man spricht von der _Prototype Chain_ 

##### Function-Constructor

Eine Art, ein Objekt zu erstellen, ist der _Function Constructor_.

```javascript
var Person = function(name, yearOfBirth, job) {
    this.name = name;
    this.yearOfBirth = yearOfBirth;
    this.job = job;
};
var john = new Person('John', 1990, 'teacher');
```

Der oben gelistete Code funktioniert, da durch den "new" Operator, die "this" Eigenschaft nicht auf den *globalen Kontext*, sondern das **neue leere Objekt** gesetzt wird. Und da der Konstruktor keinen Wert zurückliefert, wird das instanziierte Objekt als Rückgabewert gesetzt. Man hat somit ein Objekt erstellt.

Um im nächsten Schritt eine Funktion zum Objekt hinzuzufügen, welches an alle Instanzen vererbt wird, kann man die Prototyp-Eigenschaft nutzen.

```javascript
Person.prototype.calculateAge = function() {
    console.log(2020 - this.yearOfBirth);
}
```

#### Objekte erstellen

Ein anderer Weg, Objekte zu erstellen, die von einem Prototypen erben, ist die "Object.create" Methode.

```javascript
var personProto = {
	calculateAge: function() {
		console.log(2020 - this.yearOfBirth);
	}
};

var john = Object.create(personProto);
john.name = 'John';
john.yearOfBirth = 1990;
john.job = 'teacher';

var jane = Object.create(personProto, {
    name: {value: 'Jane'},
    yearOfBirth: {value: 1969},
    job: {value: 'designer'}
});
```

### Funktionen

Da Funktionen letztendlich Objekte sind, ergibt sich folgendes:

1. Funktionen sind Instanzen vom Typ Objekt
2. Funktionen verhalten sich wie Objekte
3. Funktionen können in einer Variable gespeichert werden
4. Funktionen können als Argument einer anderen Funktion übergeben werden
5. Funktionen können von anderen Funktionen als Rückgabewert ausgegeben werden

##### Funktionen als Parameter

Um zu verdeutlichen, dass sich Funktionen auch wie Objekte verhalten, gibt es ein gutes Beispiel, in dem Callback-Funktionen übergeben werden.

```javascript
var years = [1990, 1965, 1937, 2005, 1998];

function arrayCalc(arr, fn) {
    var arrRes = [];
    for(var i = 0; i < arr.length; i++) {
        arrRes.push(fn(arr[i])); // fn as a callback function
    }
    return arrRes;
}

function calculateAge(el) {
    return 2020 - el;
}

var ages = arrayCalc(years, calculateAge);
```

##### Funktionen als Rückgabewert

Ein anderes gutes Beispiel für Funktionen sind Funktionen als Rückgabewert.

```javascript
function interviewQuestion(job) {
    if (job === 'designer') {
        return function(name) {
            console.log(name + ", can you pleas explain what UX design is?");
        };
    } else if (job === 'teacher') {
        return function(name) {
            console.log(name + ", what subject do you teach?");
        };
    } else {
        return function(name) {
            console.log('Hello '+name+', what do you do?');
        };
    }
}

var teacherQuestion = interviewQuestion('teacher');
teacherQuestion('Rob');
```

##### Immediately Invoked Function Expressions (IIFE)

Um definierte Variablen zu "verstecken", falls ein Modul von anderen Programmierern eingebunden werden sollte, gibt es die _Immediately Invoked Function Expressions_. Damit bleibt alles, was in der __IIFE__ definiert wurde, im eigenen Execution Context.

```javascript
// Standard declaration
function game() {
    var score = Math.random() * 10;
    console.log(score >= 5);
}
game();
// IIFE
(function () {
    var score = Math.random() * 10;
    console.log(score >= 5);
})();
```

##### Closures

Eine innere Funktion hat immer Zugriff auf die Variablen und Parameter ihrer äußeren Funktion, auch nachdem die äußere Funktion zurückgekehrt ist.

```javascript
 function retirement(retirementAge) {
     var a = ' years left until retirement.';
     return function(yearOfBirth) {
         var age = 2020 - yearOfBirth;
         console.log((retirementAge - age) + a);
     }
 }

 var retirementUS = retirement(66);
 retirementUS(1995);
```

##### Call, Apply & Bind

Die _Call_, _Apply_ und _Bind_ Methoden erlauben das manuelle Aufrufen und das Setzen der _this_-Variable. Um dies zu verdeutlichen, hier die Beispiele.

__Call__

Wie man im nachfolgenden Beispiel sieht, kann man mit der _Call_-Methode die _this_-Variable in der Methode auf ein gewünschtes Objekt "umbiegen". Dies ist der erste Parameter in der _Call_-Methode. Dahinter kann man dann die Parameter für den Aufruf eingeben.

```javascript
const john = {
  name: 'John',
  age: 26,
  job: 'teacher',
  presentation: function(style, timeOfDay) {
    if (style === 'formal') {
      console.log(`Good ${timeOfDay}, Ladies and gentlemen! I'm ${this.name}, 
      I'm a ${this.job} and I'm ${this.age} years old.`);
    } else if (style === 'friendly') {
      console.log(`Hey! What's up? I'm ${this.name}, 
      I'm a ${this.job} and I'm ${this.age} years old. 
      Have a nice ${timeOfDay}`);
    }
  },
};

const emily = {
  name: 'Emily',
  age: 35,
  job: 'designer',
};

john.presentation('formal', 'evening');
john.presentation.call(emily, 'friendly', 'afternoon'); // Sets the this variable in the presentation function to emily
```

__Apply__

Diese Methode bietet die Möglichkeit Parameter als Array an die ursprüngliche Methode zu übergeben.

```javascript
john.presentation.apply(emily, ['formal', 'morning']);
```

__Bind__

Die _Bind_ Methode ist in der Lage, eine Kopie einer Methode zu erstellen und sie in einer Variable zu speichern. Der Vorteil ist, dass man dabei Parameter festlegen kann und beim Aufruf der kopierten Methode diesen Parameter nicht erneut eingeben muss. 

```javascript
const johnFriendly = john.presentation.bind(john, 'friendly');
johnFriendly('morning');
```