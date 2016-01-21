---
title: JavaScript Objects
lang: de
---

## Funktionen als Objekte

Auch Funktionen sind in JavaScript so genannte ```First Class Functions```. Das bedeutet, dass sie sich verhalten wie Objekte. Man kann sie on the fly generieren, einer Variable zuordnen und einer Funktion als Parameter übergeben. Das haben viele schon oft getan, zum Beispiel, wenn sie *jQuery's on document ready* Funktion genutzt haben.

{% highlight javascript %}
$(document).ready(function() {
	console.log('Das DOM ist bereit!');
});
{% endhighlight %}

Hier übergeben wir eine anonyme Funktion der Funktion ready. Diese führt unsere anonyme Funktion aus, sobald das *ready* Event im Dokument gefeuert wird.
