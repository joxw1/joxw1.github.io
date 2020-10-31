---
layout: post
title: React Lifecycle Methoden
date: 2020-10-25 14:14:45 +0300
description: Notizen über die React Lifecycle Methoden.
img: react.jpg
fig-caption: React
tags: [JavaScript, React, Lifecycle, Component, Methoden]
---
Ein wichtiger Bestandteil von React und dessen (Klassen-)Komponenten ist der Komponentenlebenszyklus. Um genauer zu sein, geht es um die Lebenszyklusmethoden, die man als Entwickler nutzen kann, um Funktionalität zwischen den Lebensphasen einer React-Komponente einzubauen.

Es gibt dabei mehrere Methoden, die unterschiedliche Zwecke haben. Nachfolgend ist ein Grafik ([Quelle](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)) eingefügt, die diesen Lebenszyklus darstellt.

![React Lifecycle Diagram](../assets/img/lifecycle_diagram.png)

Für die erste wichtige Methode *componentDidMount*, kann man sich merken, dass man in dieser Methode API-Calls feuern kann. Das bietet sich an, da vorher die Komponente gerendert wurde und im DOM eingebettet ist. In der *componentDidMount* Methode bereitet man die Komponente dann auf der "erste Update" vor. Wie man in der Swim Lane *Aktualisieren* (Grafik) sieht, wird ein Update ausgelöst wenn die Props oder der State sich aktualiseren, oder ein Update manuell forciert wird.

Wird dann ein Update ausgelöst, rendert React die Komponente auch neu. React guckt dann aber auch, was vom DOM wirklich neu geladen werden muss. In diesen Prozess kann man sich funktional auch zwischenschalten. Dafür ist die Methode *shouldComponentUpdate* gedacht. Damit lässt sich steuern, ob React die Komponente neu rendern soll.

Wenn eine Komponente nicht mehr benötigt wird und React entscheidet, eine Komponente zu entfernen, gibt es eine Art "Destruktor". Dies ist die *componentWillUnmount* Methode. In dieser Methode kann man die Funktionalität implementieren, die eine Komponente kurz bevor sie verschwindet, ausführen soll.