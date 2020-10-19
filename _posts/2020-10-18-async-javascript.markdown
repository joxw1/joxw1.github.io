---
layout: post
title: JavaScript Async
date: 2020-10-18 17:10:44 +0300
description: Notizen über Async JavaScript und Promises.
img: async.jpg
fig-caption: Asynchronität
tags: [JavaScript, Grundlagen, Async, Promises]
---
Ein großer Bestandteil in der JavaScript-Programmierung ist die Behandlung von Web-Requests und dessen Responses. Da dies meist in Echtzeit passiert, ist es unerlässlich einen asynchronen Abarbeitungsfluss zu programmieren. Das bedeutet, man kann die Antwort eines Requests erst verarbeiten, wenn die Antwort auch da ist. In diesen Notizen sollen die asynchronen Prozessabläufe in JavaScript festgehalten werden.

### Synchron vs. Asynchron

Ein synchroner Prozessablauf ist ein standardmäßiges Programm, welches Zeile für Zeile abgearbeitet wird. Es gibt dabei nur einen einzelnen Thread, welcher im Browser abgearbeitet wird. Im Gegensatz dazu, versucht man mit einem asynchronen Prozessablauf, bestimmten Code im Hintergrund laufen zu lassen bis ein Ergebnis berechnet wurde. Dafür übergibt man meist eine Callback-Funktion, die bei Fertigstellung aufgerufen wird. So wird das Programm an keiner Stelle blockiert, sondern läuft nahtlos weiter.

### Promises

Ein **Promise** ist ein Objekt, welches den Überblick darüber behält, ob ein Event bereits abgeschlossen ist oder nicht. Außerdem entscheidet das Objekt, was passieren soll, wenn das zu überwachende Event abgeschlossen ist. Dabei beinhaltet das **Promise** den Wert, der vom Event zurückgegeben wird.

Dadurch, dass ein Promise davon abhängt, ob ein Event abgeschlossen ist, gibt es auch unterschiedliche Zustände davon. Bevor ein Event ausgelöst wird, ist ein Promise noch im Zustand *Wartend (Pending)*. Ist das Event dann abgeschlossen, ist das Promise im Zustand *Erledigt/Gelöst (Settled/Resolved)*. Je nachdem, ob das Event erfolgreich war, ist das Promise in einem anderen Zustand nachdem das Event erledigt wurde. Wenn das Event erfolgreich abgeschlossen wurde, ist das Promise im Zustand *Erfüllt (Fulfilled)*. Falls ein Fehler auftritt ist, es im Zustand *Zurückgewiesen (Rejected)*.

Durch diesen Zustandsgraphen ergibt sich auch ein bestimmter Terminus, wie Promise genutzt werden. Wird ein Promise angelegt, wird es **produziert**. Das bedeutet, man legt es an, ohne den Ausgangswert zu kennen. Wenn der Ausgangswert klar ist, kann dieses Promise **konsumiert** werden.

```javascript
const getIds = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(
      [239, 123, 994, 122, 32]
    );
  }, 1500);
});
getIds
  .then((ids) => {
  console.log(ids);
	})
  .catch((error) => {
  console.error('Error!');
	});
```

### Async/Await

In ES8 (ES2017) wurde eine neue Syntax eingeführt, um das konsumieren von Promises zu vereinfachen. Mit dem **async** Schlüsselwort können asynchrone Funktionen deklariert werden, in welchen das **await** Schlüsselwort genutzt werden kann. Mit diesem Schlüsselwort wartet der Prozess, bis das Promise, auf welches gewartet wird, im Status *Erledigt* ist.

```javascript
async function getRecipesAW() {
  const ids = await getIds;
  const recipe = await getRecipe(ids);
  const related = await getRelated(recipe.publisher);
  return related;
}
getRecipesAW().then(value => console.log(value));
```