---
layout: post
title: Django Syntax highlighting in TextMate
date: '2009-01-09 18:06:00'
---

The default [TextMate](http://macromates.com/) installation doesn't include special Syntax Hightlighting for Django Templates. They provide a special bundle in their SVN repository though. To install the bundle fire up a terminal and enter

```bash
svn --username anon --password anon co http://macromates.com/svn/Bundles/trunk/Bundles/Python%20Django%20Templates.tmbundle/
```

Now copy the file to the following directory (in your home dir)

```bash
~/Library/Application Support/TextMate/Pristine Copy/Bundles
```

The bundle also includes some nice code completion. For example if you type `if` and hit tab it will automatically create and if block in your template.
