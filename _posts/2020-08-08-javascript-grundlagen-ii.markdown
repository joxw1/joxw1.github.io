---
layout: post
title: JavaScript Grundlagen II
date: 2020-08-08 15:24:25 +0300
description: Zweiter Teil der JavaScript Grundlagen. Es wird noch ein Nachtrag zu Operatoren geliefert und es werden Verzweigungen und Logik eingeführt.
img: js-2.png
fig-caption: JavaScript Grundlagen II
tags: [JavaScript, Grundlagen, Verzweigungen, Logik]
---
Im zweiten Teil der JavaScript-Grundlagen Notizen, basierend auf dem Kurs von [Jonas](https://www.udemy.com/course/the-complete-javascript-course/) geht es hauptsächlich um Verzweigungen und Operatorlogik. Zunächst jedoch noch ein Nachtrag zu den Basisoperatoren.

#### Operatorprioritäten

Die Operator-Priorität bestimmt, wie die Operatoren untereinander geparst werden. Operatoren mit höherer Priorität werden zu Operanden von Operatoren mit niedrigerer Priorität.

Die Operatorprioritäten sind in einer [Tabelle](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence>) aufgeführt. In dieser Tabelle wird zudem deutlich, dass nicht nur die Priorität eine Rolle spielt, sondern auch die "Assoziativität". Diese gibt an, ob Operatoren von "links-nach-rechts" oder "rechts-nach-links" ausgeführt werden.

##### Beispiel

```javascript
var x, y;
x = y = (2 * 10) * 2 + 2; 
console.log(x, y) // x ist nicht 'undefined', da von rechts nach links gelesen wird
```

### Verzweigungen

Verzweigungen werden genutzt um den Programmfluss zu ändern. Man kann z.B. je nach Wert einer Variable die nächste Unterfunktion aufrufen oder Variablen neue Werte zuweisen.

```javascript
var firstName = 'John'
var civilStatus = 'single'

if(civilstatus === 'married') {
    console.log(`${firstName} is married`);
} else {
    console.log(`${firstName} is single`)
}
```

Innerhalb der Bedingungen kann eine beliebig komplexe boolsche Logik stecken. Man muss dabei jedoch auch immer die Operatorpriorität beachten. 

``` javascript
var firstName = 'John';
var age = 16;

if(age < 13) {
    console.log(`${firstName} is a boy.`)
} else if(age >= 13 && age < 20) {
    console.log(`${firstName} is a teenager.`)
} else {
    console.log(`${firstName} is a man.`)
}
```

##### Ternäre Operatoren

Der ternäre Operator erlaubt das Schreiben eines If-Else-Statement in einer Zeile.

``` javascript
var firstName = 'John';
var age = 16;
var drink = age >= 18 ? 'beer' : 'juice';
console.log(`${firstName} drinks ${drink}.`)
```

##### Switch-Statement

Eine Alternative zum If-Else-Statement ist das Switch-Statement. Es wird vorallem bei einem großen Entscheidungsbaum mit simplen Bedingungen (Conditionals) verwendet.

```javascript
var job = 'teacher';
switch(job) {
    case 'teacher':
        console.log(`${firstName} is a teacher.`);
        break;
    case 'driver':
        console.log(`${firstName} is a driver.`);
        break;
    default:
        console.log(`${firstName} is something else.`)
}
```

##### Truthy & Falsy

Truthy- und Falsy-Werte, sind bestimmte Werte die in einer Verzweigung entweder wahr oder falsch sind. So ist "undefined" z.B. ein Falsy-Wert, da in einer If-Bedingung dieser Wert als "false" interpretiert werden würde.

```javascript
// falsy: undefined, null, 0, '', NaN
// truthy: NOT falsy :-)

var height;
if (height){
    console.log('Variable defined.')
} else {
    console.log('Variable is not defined.')
}
```

Worauf man jedoch achten muss ist, dass auch die 0 ein Falsy-Wert ist. Würde man im o.g. Beispiel der Variable "height" eine 0 zuweisen, wäre der Wert definiert. Trotzdem würde der Else-Block ausgeführt werden, da 0 nicht "wahr" ist. Dadurch ergibt sich in JavaScript ein Muster, welches oft angewendet wird. Möchte man den Wert einer Variable überprüfen, sollte man erst überprüfen, ob sie definiert ist.

```javascript
var height = 0;
if(height || height === 0) {
	console.log('Variable defined.')
} else {
	console.log('Variable is not defined.')
}
```

##### Gleichungsoperatoren

In JavaScript gibt es zwei unterschiedliche Operatoren, um eine Gleichung zu prüfen. 

```javascript
var height = 23;
if (height == '23') {
    console.log('The == operator does type coercion.')
    // Wird ausgeben, da der Typenzwang angewendet wird.
}
if (height === '23') {
    // Wird nicht ausgeführt, da auch der Typ gleich sein muss. (Best Practice)
}
```