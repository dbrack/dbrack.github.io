---
layout: post
title: Useful Scripts
date: '2009-01-08 17:49:00'
---

Here are two shell scripts I use pretty often. I 've created an alias in my `.zshrc` to easily access the commands. The first one is called `pylink`. It creates a symlink from a folder to the python-path.

```language-bash
ln -s `pwd`/$1 `python -c "from distutils.sysconfig import get_python_lib; print get_python_lib()"`/$1
```

The second one is called `delpyc`. It deletes all .pyc and .pyo files in a folder and all it's subfolders.

```language-bash
find . \( -name "*.pyc" -o -name "*.pyo" \) -exec rm -v {} \;
```

I found both scripts somewhere on [EricFlo's](http://www.eflorenzano.com) site.