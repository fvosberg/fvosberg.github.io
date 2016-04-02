---
title: Die Tücken mit dem Z-Index
lang: de
---

Bei CSS gibt es ein paar Tücken. Wer kennt das nicht? Der z-Index ist an sich etwas sehr Simples. Manchmal will er aber einfach nicht so wie man sich das vorstellt. Um das in Zukunft zu vermeiden erkläre ich hier die grundsätzlichen Regeln, die bestimmen, welche Elemente welche überdecken.

Dazu führen 2 Parameter: Das sind die Stack Order, also die Reihenfolge der Elemente, und der Stack Context, also das Umfeld in dem das entschieden werden muss.

## Die Stack Order

Die Stack Order ist der intuitivere Parameter und seltener das Problem. Um sie getrennt von dem Stack Context zu betrachten, nehmen wir erst einmal nur Schwesternelemente, die direkte Kinder des Body-Tags sind. Denn Elemente auf einer Ebene sind zwangsläufig im gleichen Kontext.

### Positionierte Elemente über Elementen im Fluss

Sobald sich 2 Elemente überlappen sind positionierte Elemente vor nicht positionierten.

*Nicht positioniert* bedeutet in diesem Fall, dass der Wert der CSS-Eigenschaft `position` nicht `static` ist.

<iframe width="100%" height="270" src="//jsfiddle.net/fvosberg/983w12gg/1/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Den Sinn dahinter kann man intuitiv nachvollziehen: Wenn man ein Element bewusst aus dem Fluss nimmt, will man es auch sehen können.

### Höherer Z-Index über Niedrigerem

Um die Reihenfolge von positionierten Elemnten steuern zu können gibt es die CSS-Eigenschaft `z-index`. Diese greift allerdings nur bei positionierten Elementen, deswegen ist diese das zweite Kriterium bei der Steuerung welche Elemente welche überlappen. Elemente mit einem höheren Z-Index überlappen Elemente mit einem Niedrigeren.

<iframe width="100%" height="270" src="//jsfiddle.net/fvosberg/983w12gg/2/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### Später definierte Elemente vor früher definierten

Ist der Z-Index gleich, zum Beispiel weil er nicht definiert ist, entscheidet die Reihenfolge im HTML.

<iframe width="100%" height="270" src="//jsfiddle.net/fvosberg/983w12gg/7/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Das grüne und das rote `DIV` haben sind beide positioniert und haben den gleichen `z-index`. Das Grüne überlagert das Rote, weil es später definiert wurde.

### Zusammenfassung

<iframe width="100%" height="270" src="//jsfiddle.net/fvosberg/983w12gg/5/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### Die Positionierung ist wichtiger als der Z-Index und die Reihenfolge

Das gelbe `DIV` liegt ganz unten, obwohl es den höchsten Z-Index hat und als letztes definiert worden ist.

#### Der Z-Index ist wichtiger als die Reihenfolge

Das grüne und das rote `DIV` werden nach dem Blauen definiert, haben aber einen niedrigeren Z-Index.

#### Die Reihenfolge entscheidet als Letzte

Rot und grün haben den gleichen Z-Index, da das Grüne aber nach dem Roten definiert wurde hat es die höhere Priorität.

#### Sonderfall: Unpositionierte Elemente

Bei unpositionierten Elementen gilt an sich auch die Regel, dass später definierte Elemente über früher definierten angezeigt werden, allerdings überdeckt deren Hintergrund nicht die Schrift des Anderen, sie sind so zu sagen *verwoben*.

<iframe width="100%" height="210" src="//jsfiddle.net/fvosberg/2erg45fd/2/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Hier sieht man, dass der Text des ersten Absatzes, der über ihn hinaus geht, zwischen dem Hintergrund und dem Text des Zweiten angezeigt wird. Möchte man diesen Effekt vermeiden muss man eins der Elemente positionieren.

## Der Stack Context

Hier passieren häufiger Fehler weil es weniger intuitiv und weniger bekannt ist. Die *Stack Order* gilt innerhalb eines *Stack Context*, der Kontext ist also wichtiger. Die Kontexte selbst befinden sich zueinander wieder in einer Stack Order.

Ein neuer Kontext wird durch folgende Elemente erzeugt

* Durch das Wurzelelement einer Seite (`<html></html>`)
* Wenn ein Element positioniert ist *und* es einen `Z-Index` definiert hat
* Wenn ein Element eine CSS-Eigenschaft hat, welche sein Rendering grundlegend ändert.
    * Opacity unter 1
    * Transforms
    * Filters
    * CSS-Regions
    * Paged Media

Die ersten Zwei sind offensichtlich und logisch. Die darauf Folgenden sind die größte Tücke in diesem Zusammenhang. Sollte der Z-Index also nicht funktionieren, so wie ihr es erwartet, sucht nach solchen CSS-Eigenschaften.

### Beispiel

<iframe width="100%" height="210" src="//jsfiddle.net/fvosberg/c4gbLo07/embedded/result,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Das rote `DIV` ist vor allen anderen, da sein Elterndiv das Einzige ist, welches keinen neuen Kontext erzeugt. Deswegen konkurriert es mit den anderen Elterndivs. Diese haben aber alle keinen Z-Index, abgesehen vom `#first`. Es ist aber später definiert und hat deshalb eine höhere Priorität.

Das grüne `DIV` ist unter dem Blauen, obwohl es einen höheren Z-Index hat, weil sein Elternelement durch die `Opacity` einen neuen Kontext erzeugt. Dieser Kontext hat eine niedrigere Priorität als die des Blauen, weil er vor ihm definiert wurde.
