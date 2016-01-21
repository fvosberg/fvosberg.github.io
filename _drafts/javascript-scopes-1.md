---
title: JavaScript Scopes - Teil 1
lang: de
---

Das hier ist ein der erste Teil eines Grundlagenartikel über Scopes in JavaScript. Am Anfang werde ich einfachste Grundlagen wiederholen, die dann immer komplizierter werden. Im zweiten Teil werde ich dadurch Closures erklären, die bei vielen für Kopfschmerzen sorgen. 

## Was ist ein Scope?

Ein Scope ist der Bereich, in dem eine Variable deklariert ist, also verfügbar. In JavaScript deklarieren wir Variablen mit dem ```var```-Statement. Ab ECMAScript 6 gibt es durch das ```let```-Statement eine zweite Möglichkeit. Diese betrachten wir erst im zweiten Teil. 

## Funktionen sind die Grenzen von Scopes

Um einen neuen Scope zu eröffnen, reicht eine einfache Funktion.

{% highlight javascript %}
var a = 5;

console.log(a); // => 5

function test() {
	var a = 3;
	console.log(a);
}

console.log(a); // => 5
test(); // => 3
console.log(a); // => 5
{% endhighlight %}

## Scopes sind verschachtelt

Ist eine Variable im derzeitigen Laufzeitkontext (Scope) nicht deklariert, wird sie im ihn umgebenen Kontext gesucht. Das nennt sich *Scope-Chain*. Jeder Scope hat eine Kette bis zum globalen Kontext.

{% highlight javascript %}
var a = 5;
function test() {
	console.log(a);
}

console.log(a); // => 5
test(); // => 5
{% endhighlight %}

Was auch bedeutet, dass Variablen aus einem tieferen Kontext heraus verändert werden können.

{% highlight javascript %}
var a = 5;
function test() {
	console.log(a); // => 5
	a = 3;
	console.log(a); // => 3
}

console.log(a); // => 5
test(); // Siehe in der Funktion
console.log(a); // => 3
{% endhighlight %}

## Parsetime vs Runtime

{% highlight javascript %}
var a = 5;
function test() {
	console.log(a);
	var a;
}

console.log(a);
test();
console.log(a);
{% endhighlight %}

Mit dem bis jetzt Gelernten ist es richtig davon auszugehen, dass drei mal der Wert 5 geloggt wird. Das Problem ist, dass die Variablendeklaration (```var a;``` in der Funktion ```test```) nicht erst zur Laufzeit vom Interpreter ausgewertet, sondern schon vorher von dem Parser. Das bedeutet, dass der Interpreter in dem Scope der Funktion *test* eine eigene Variable mit dem Namen *a* hat. Diese hat während des Logs noch keinen Wert zugewiesen bekommen und hat somit den Wert ```undefined```. Deswegen erscheint in der Konsole Folgendes:

{% highlight javascript %}
5
undefined
5
{% endhighlight %}

Um Verwirrungen zu vermeiden empfehle ich Euch die ```var```-Statements immer an den Anfang des Scopes zu setzen. Denn folgender Code hat die gleiche Ausgabe und das kann zu Missverständnissen führen:

{% highlight javascript %}
var a = 5;
function test() {
	console.log(a);
	var a = 3;
}

console.log(a);
test();
console.log(a);
{% endhighlight %}

## Schlusswort

Sehr gut, der Grundstein ist gelegt. Diese einfachen Grundlagen verhindern schon einige Kopfschmerzen und wildes Rumprobieren. Im zweiten Teil werden wir uns noch weiter mit Scopes beschäftigen, insbesondere mit Closures. Insbesondere letztere sorgen für einen wesentlichen besseren Code, auch wenn er noch prozedural geschrieben wird.
