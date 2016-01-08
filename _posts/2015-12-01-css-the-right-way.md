---
title: CSS the right way
lang: de
---

Fast alle Webentwickler *beherrschen* CSS mehr oder weniger ... sagen sie zumindest. Was sie meinen ist, dass sie es mit Hilfe von CSS schaffen, dass die Internetseite fast so aussieht wie sie es gerne hätten. Insbesondere bei großen Projekten, die dann auch noch *responsive* umgesetzt werden sollen, passiert es schnell, dass das CSS unübersichtlich wird, unerwartete Seiteneffekte hat und nicht mehr einfach zu händeln ist. Da spielen aus meiner Sicht zwei große Faktoren eine Rolle, die ich mehr als nachvollziehen kann. 

<img src="https://i.imgur.com/Q3cUg29.gif" alt="CSS" />

Erstens: CSS kann eine Bitch sein. Wie oft dachte man schon, man hätte `float` verstanden und dann macht es nicht was es soll. 

Zweitens: Die Meisten wollen sich beim CSS-Schreiben keine großen Gedanken machen. Ich will, dass die Box 100px breit ist und einen grünen Hintergrund hat, also schreibe ich erst einen Selektor, der diese Box anspricht und dann setze ich die zwei entsprechenden CSS Regeln. Das funktioniert am Anfang auch super, irgendwann wird es aber unübersichtlich, wenn man nicht einige Regeln dabei beachtet. (Wenn ihr das schon intuitiv tut, gut für Euch)

## !important is evil

Ein Thema bei dem es mir schwer fällt die Fassung zu behalten und sachlich zu diskutieren. Solange Euch keiner zwingt, benutzt kein `!important`. Die Rangordnung von CSS Regeln definiert sich wie folgt (aufsteigend definiert, als spätere sind wichtiger und überschreiben vorherige):

* Eine Anweisung mit dem Tag (a, h1, p) als Selektor
	* Eine spätere Anweisung
	* Eine Anweisung mit mehr Selektoren
* Eine Anweisung mit einer Klasse als Selektor
	* Eine spätere Anweisung
	* Eine Anweisung mit mehr Selektoren
* Eine Anweisung mit einer ID als Selektor
	* Eine spätere Anweisung
	* Eine Anweisung mit mehr Selektoren
* Eine Anweisung als inline-style

Selbst wenn man sich dessen nicht so bewusst wird ist das nur logisch. Je spezifischer eine Anweisung, umso genauer, also auch umso wichtiger ist sie. *Important* sorgt dafür, dass diese Hierarchie einmal übersprungen wird. Ein Selector mit einem HTML-Tag (a) und einem Important ist wichtiger, als eine Inline-Anweisung oder eine Anweisung mit einer ID. Wenn man es also auf die Spitze treiben möchte schreibt man *!important* in einen Inline-Style.

## Nicht zu spezielle Regeln

Meistens wird der Fehler begangen, dass Selektoren von CSS-Regeln zu speziell sind, bedeutet, dass er oft aus zu vielen Selektoren besteht. Ein kleines Beispiel:

<iframe width="100%" height="400" src="//jsfiddle.net/fvosberg/khtpme15/4/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Wir wollen einen Abstand zwischen dem Absatz in der Section und dem Text in der Aside. Deswegen haben wir folgende CSS Regel geschrieben:

{% highlight css %}
section p {
    margin-right: 10px;
}
{% endhighlight %}

Der Selektor erscheint nicht besonders speziell, er ist trotzdem zu speziell. Angenommen wir wollten jetzt eine UL in die Sektion einfügen, dann müssten wir eine weitere Regel im CSS für diese Liste schreiben, damit diese das selbe Margin bekommt. Ergo ist folgende Regel schon deutlich besser:

{% highlight css %}
section > * {
	margin-right: 10px;
}
{% endhighlight %}

Jetzt können wir beliebige Elemente in die Section einfügen, alle bekommen das gleiche Margin. Dennoch überschreiben wir vielleicht das margin einer UL, das sie immer haben soll, dadurch. Nicht nur, dass wir eigentlich nicht einen Absatz in der Section stylen wollen, wir wollen eigentlich nicht die Inhalte der Section verändern, sondern der Section einen Rand verpassen. Das geht mit padding weitaus einfacher und besser

{% highlight css %}
section {
	padding-right: 10px;
}
{% endhighlight %}

Das scheint ein triviales Beispiel, aber es veranschaulicht im Einfachen die Problematik.

## Relative Angaben nutzen

Insbesondere beim Responsive Design ist es nötig relative Angaben zu machen, damit man nicht für jeden "Breakpoint" komplett neues CSS schreiben muss. Aber auch beim schreiben von Webseiten mit einer festen Breite kann es helfen, weil man sich besser bewusst wird, wie das Aussehen der Elemente zusammenhängt.

Damit sind aber nicht nur Prozentangaben bei Breiten und Höhen gemeint, sondern insbesondere auch bei der Schrift. Mit der Größenangabe *em* gibt man an, wie groß die Schrift im Verhältnis zum Elternelement sein soll. 

Als einfaches Beispiel nehmen wir Überschriften. Wir wollen, dass eine h1 immer 1.3 mal so groß ist wie der Fließtext auf der gleichen Ebene. Wenn wir für den Fließtext keine Angabe machen ist die Schriftgröße durch das Elternelement definiert. Dann brauchen wir nur noch für die h1 eine Angabe von *1.3 em*. Jetzt ist es egal an welcher Stelle wir diese einfügen, sie ist bereits im richtigen Verhältnis zum Fließtext, auch, wenn wir auf einem Smartphone die Schriftgröße erhöhen.

Wenn wir ausschließlich relative Schriftgrößen verwenden ist es nicht nur möglich die Schrift auf der ganzen Seite um 10% zu vergrößern, indem wir dem Body eine *font-size* von *1.1em* geben sondern auch, dass Menschen mit einer Seeschwäche einen einfacheren Zugang zur Internetseite bekommen.

## Résumé

Macht euch Gedanken wie die Regel wirklich lautet, die ihr für das Styling braucht. Je weniger CSS ihr benötigt umso besser. Macht euch auf der anderen Seite aber nicht verrückt. 
