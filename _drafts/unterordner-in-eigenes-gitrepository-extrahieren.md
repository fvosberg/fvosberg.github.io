---
title: Unterordner in ein eigenes Gitrepository extrahieren
lang: de
---

Es seit Git ***TODO*** eine eingebaute Möglichkeit, die super funktioniert, wenn man nur einen Branch als `master` Branch exportieren möchte. Möchte man alle Git Branches exportieren sollte man den Weg mit `git filter-branch` gehen.

## Git Subtree

## Git Filter Branch

{% highlight bash %}
# Git Repository in einen eigenen Ordner klonen. Dort werden wir später nur noch das neue Repository haben
git clone altesRepository ordnerFürNeuesRepository

# move to a headless state
# in order to delete all branches without issues
git checkout --detach

# delete all branches
git branch | grep --invert-match "*" | xargs git branch -D

# Isolate dir1 and recreate branches
# --prune-empty removes all commits that do not modify dir1
# -- --all updates all existing references, which is all existing branches
git filter-branch --prune-empty --subdirectory-filter dir1 -- --all
{% endhighlight %}
