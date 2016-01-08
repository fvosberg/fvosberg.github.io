---
title: Gitignore alles außer .htaccess (rekursiv)
lang: de
---

Heute hatten wir das Problem, dass wir einen Ordner mit User-Uploads von der Git Versionierung ausschließen wollten, allerdings kann es sein, dass irgendwo unterordner durch eine .htaccess Datei geschützt werden sollen. Anstatt diese Restriktionen in die übergeordnete .htaccess Datei zu verlagern haben wir uns dazu entschlossen mit Hilfe der .gitignore tatsächlich alle Dateien, abgesehen von der .htaccess, auszuschließen. Damit das klappt muss man nur daran denken Ordner nicht zu ignorieren, da es sonst nicht rekursiv funktioniert. 

Hier der Code:

{% highlight ApacheConf %}
subfolder/**/*
!subfolder/**/.htaccess
!subfolder/**/*/
{% endhighlight %}
