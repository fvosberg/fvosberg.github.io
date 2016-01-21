---
title: Content Collections für Seiten in Neos anlegen
lang: de
---

Sites/Your.SitePackage/Configuration/NodeTypes.yaml
{% highlight yaml %}
'TYPO3.Neos.NodeTypes:Page':
  childNodes:
    'footer':
      type: 'TYPO3.Neos:ContentCollection'
{% endhighlight %}

Durch unsere neue Seitenkonfiguration fehlen

{% highlight bash %}
./flow node:repair
{% endhighlight %}
