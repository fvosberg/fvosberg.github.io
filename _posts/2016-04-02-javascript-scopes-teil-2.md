---
title: JavaScript Scopes - Teil 2
lang: de
---

Das hier ist die Forsetzung von [JavaScript Scopes - Teil 1]({% post_url 2016-01-21-javascript-scopes-teil-1 %}). Insbesondere geht es um Closures, welche eine unfassbare Hilfe dabei sind JavaScript code zu organisieren und Scopes auszunutzen.

## Parameter

Wenn man einen Parameter für eine Funktion definiert, dann sagt man damit dem JavaScript Parser, dass er diesen als Variable in der Funktion deklarieren soll. Sie existiert also nur in dieser Funktion und eventuell in Scopes darunter.

{% highlight javascript %}
var a = 1;

function test(a) {
	console.log(a);
	a = 3;
}

console.log(a); // => 1
test(2); // => 2
console.log(a); // => 1
{% endhighlight %}

Dewegen verändert sich die Variable a außerhalb der Funktion auch nicht.

## Function Statements vs Function Expressions

Unabhängig von Funktionen kennt der JavaScipt Parser zwei Arten von Anweisungen. Statements und Expressions. Statements werden zur Parse-Time ausgewertet und Expressions erst zur Runtime. Im Groben gilt die Faustregel, dass Anweisungen, die einen Rückgabewert haben Expressions sind, Statements hingegen arbeiten "nur".

{% highlight javascript %}
if () {} // Statement
var a; // Statement
a = b; // Expression
{% endhighlight %}

Auch bei der Definition einer Funktion kann man sich entscheiden, ob man sie in Form eines Statements oder einer Expression deklariert.

{% highlight javascript %}
// Statement; kein Rückgabewert bei Interpretation
function test () {
	return 'foobar';
}

// Expression; Rückgabewert, die Funktion selbst
var test = function () {
	return 'foobar';
}
{% endhighlight %}

Letzteres ist streng genommen ein Statement, nämlich die Deklaration der Variablen test, und eine Expression, nämlich die Zuweisung der Funktion zu dieser Variablen. In beiden Fällen lassen sich die Funktionen mit ```test()``` ausführen. Der Unterschied der beiden Definitionen ist, dass das Statement komplett zur Parsetime ausgeführt wird, was bedeutet, dass es beim Parsing länger dauert, die Funktion dann schon den Speicher belegt, diese dann aber in ihrem Scope ausführbar ist. Beim Zweiten wird von dem Parser nur die Variable ```test``` in ihrem Scope deklariert. Der Rest geschieht es zur Runtime.

<iframe width="100%" height="240" src="//jsfiddle.net/fvosberg/qoqfum5c/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
<iframe width="100%" height="240" src="//jsfiddle.net/fvosberg/qoqfum5c/1/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Dadurch sieht man, dass man Funktionen getrennt von Variablen betrachten kann. Die Variablen benutzt man nur zur Ausführung. Daraus folgt auch, dass Funktionen genau wie Variablen in einem Scope existieren.

<iframe width="100%" height="260" src="//jsfiddle.net/fvosberg/ejffzrnb/1/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## Closure

Funktionen können also unabhängig von Variablen existieren und man kann sie welchen Zuweisen. Außerdem definieren Funktionen neue Scopes.
Als Beispiel programmieren wir eine Funktion um Streichhölzer zu ziehen. Wir sind eine Gruppe von 3 Leuten, haben also 3 Streichhölzer, eines davon ist kürzer als die anderen. Wurden alle drei Streichhölzer gezogen gibt die Funktion ```undefined``` zurück.

Als erster Ansatz:

<iframe width="100%" height="260" src="//jsfiddle.net/fvosberg/syd2g09j/2/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Diese Implementierung hat das Problem, dass die Variable mit den Strohhalmen im globalen Kontext liegt. Das für zu zwei Nachteilen:

0. Die Variable kann unerwartet verändert werden und die Funktion tut nicht mehr das, was man erwartet.
0. Der Code ist unübersichtlicher, weil man nicht weiß ob es Absicht ist und man danach suchen muss, ob die Variable woanders gewollt verändert wird.

Um einen neuen Scope zu erzeugen, in dem wir uns bewegen können, schreiben wir eine Funktion als wrapper um alles herum und geben die alte Funktion als Rückgabewert zurück. Diese hat weiterhin Zugriff auf den Scope, in dem sie definiert worden ist. Genau das nennt man Closure.

<iframe width="100%" height="260" src="//jsfiddle.net/fvosberg/syd2g09j/3/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Das ganze kann man noch etwas aufräumen. Das Erste was auffällt ist, dass wir den Wrapper ausführen müssen, um unsere Closure (die Funktion drawStraw) zu bekommen. Das kann man abkürzen, in dem man eine anonyme Funktion als Wrapper verwendet. Diese Funktion weisen wir auch keiner Variablen zu, sondern wir führen sie direkt aus.

<iframe width="100%" height="260" src="//jsfiddle.net/fvosberg/syd2g09j/4/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Damit das funktioniert, müssen wir die anonyme Funktion in runde Klammern packen. Der JavaScript Parser parst den Code zeichenweise. Durch die runde Klammer weiß er, dass wir eine Expression definieren. Würden wir das nicht tun, würde er bei dem Wort function ein Statement erwarten, welches er dann natürlich nicht ausführen kann.

Außerdem können wir noch darauf verzichten der eigentlichen drawStraw-Funktion einen Namen zu geben, da wir sie nur zurückgeben wollen.

<iframe width="100%" height="260" src="//jsfiddle.net/fvosberg/syd2g09j/5/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### Zusammenfassung

0. Wir definieren eine anonyme Funktion, um einen Scope zu definieren.
0. Diese führen wir direkt aus. Dabei geschehen 2 Dinge
	0. Wir initilisieren unsere Variablen, die nicht in der eigentlichen Funktion stecken sollen.
	0. Wir geben die eigentliche Funktion mit der Logik zurück.
0. Diese zurückgegebene Funktion schreiben wir in eine Variable, um sie nachher ausführen zu können.

## Das let Statement

Ab ECMAScript 6 gibt es eine Alternative zur Variablen Deklaration mit ```var```. Im Gegensatz zu ```var``` deklariert man mit ```let``` Variablen nicht in einem Scope, der nur durch eine Funktion abgegrenzt wird, sondern auch schon durch einen Block. Ein Block wird durch geschweifte Klammern abgegrenzt. Zum Beispiel in Schleifen oder ```if``` Blöcken.

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

## Schlusswort

Dank Scopes und Closures ist es möglich, JavaScript Code zu kapseln. Dadurch kann man seinen Code deutlich leichter lesbar machen und robuster gestalten. Bei vielen Websites reicht diese Kapselung aus, um im Frontend Ordnung zu halten. Bei aufwändigeren JavaScript-Anwendungen kann es aber Sinn ergeben seinen Code noch weiter mit Hilfe von Prototypen zu organisieren.
