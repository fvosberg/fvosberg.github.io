---
title: JavaScript Scopes - Teil 2
lang: de
---

## Blockscope

Gibt es nicht

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

## Closure

## Das let Statement
