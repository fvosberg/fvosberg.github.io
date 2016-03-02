---
title: JavaScript Scopes - Teil 2
lang: de
---

## Parameter

{% highlight javascript %}
var a = 1;

function test(a) {
	console.log(a);
	a = 3;
}

console.log(a);
test(2);
console.log(a);
{% endhighlight %}

## Objekte als Scope

{% highlight javascript %}
var a = 1;

function test() {
	this.a = 5;
	console.log(a); // => 1
	console.log(this.a); // => 5
}

console.log(a);
test();
console.log(a);
{% endhighlight %}

## Nutzung von Attributen von Funktionen


{% highlight javascript %}
var executing = 0;

function test() {
	if(!executing) {
		executing = 1;
		// do stuff
		executing = 0;
	}
}
{% endhighlight %}

{% highlight javascript %}
function test() {
	if(!this.executing) {
		this.executing = 1;
		// do stuff
		this.executing = 0;
	}
}
{% endhighlight %}

## Function Statements vs Function Expressions

Statements arbeiten nur und Expressions geben einen Wert zurück. 

if () {} ist ein Statement
var a = b ist eine Expression

Statement (kein Rückgabewert)
function test () {
}

Expression
var test = function () {
}

## Closure

## Das let Statement

Wir haben schon gelernt, dass das ```var``` Statement den Variablen-Scope durch Funktionen begrent. Ab ECMAScript 6 gibt es aber auch noch eine Alternative, das ```let```-Statement. Dieses setzt den Variablen-Scope aufgrund von Blöcken. Ein Block wird meist durch geschweifte Klammern begrenzt. Hier ein Beispiel

{% highlight javascript %}
var a = 1;
function test() {
	var b = 2;
	if(true) {
		let c = 3;
		console.log(a); // => 1
		console.log(b); // => 2
		console.log(c); // => 3
	}
	console.log(a); // => 1
	console.log(b); // => 2
	console.log(c); // => undefined
}
console.log(a); // => 1
console.log(b); // => undefined
console.log(c); // => undefined
{% endhighlight %}
