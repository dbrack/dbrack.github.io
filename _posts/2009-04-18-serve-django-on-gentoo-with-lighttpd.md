---
layout: post
title: Serve Django on Gentoo with lighttpd
date: '2009-04-18 14:09:00'
---

In this post you'll find a step by step guide on how you can serve your [Django](http://www.djangoproject.com) site on [Gentoo](http://www.gentoo.org), using [lighthttpd](http://www.lighttpd.net) and [FastCGI](http://www.fastcgi.com). Even though this guide is written specifically for Gentoo, it might also help you if you use another Linux distribution.

Let's start by installing the required packages. As root, run the following command (flup is required for Django's init.d script)

```bash
emerge flup lighttpd
```

Next, go and grab yourself a copy of Django's [InitdScript for Gentoo](http://code.djangoproject.com/wiki/InitdScriptForGentooLinux) and give it an appropriate name. Mine is called `django-fastcgi`. Just to mention it, a version for other Linux distributions is also available [here](http://code.djangoproject.com/wiki/InitdScriptForLinux). The script is pretty self-explaining. You only have to change the DJANGO_SITES and SITES_PATH variables. The `DJANGO_SITES` variable is the name of your Django project, and the `SITES_PATH` variable is the path to your project (without a trailing slash I think).
Let's assume the absolute path to your project is `/var/www/myproject/`. The configuration for this setup would look like this

```python
DJANGO_SITES="myproject"
SITES_PATH=/var/www
RUNFILES_PATH=/var/django/run
HOST=127.0.0.1
PORT_START=3000
```

Now you can copy the script to the init.d directory by running

```bash
cp scriptname /etc/init.d/
```

Make sure the script is executable

```bash
chmod +x /etc/init.d/scriptname
```

Try to start the fast-cgi process like this

```bash
/etc/init.d/scriptname start
```

If you don't see any errors showing up, you can add the script to the default runlevel to start the script automatically if you have to reboot your server or so. In order to do so, simply run

```bash
rc-update add scriptname default
```

Alright, still with me? Cause we're not done yet.
We can now configure our lighttpd server. With your editor of choice, open up lighty's config file. You can find it in `/etc/lighttpd/lighttpd.conf`.
Make sure you have the following modules enabled

```python
server.modules = (
	"mod_rewrite",
	"mod_redirect",
	"mod_alias",
	"mod_access",
	"mod_fastcgi",
	"mod_accesslog"
)
```

Now let's configure lighty to work with our fast-cgi process. Somewhere at the bottom of the configuration file, add the following part

```bash
$HTTP["host"] =~ "(^|\.)yourdomain\.com$" {
    server.document-root = "path/to/your/django/project" # same path as the SITES_PATH in the fastcgi script
    server.errorlog = "/path/to/your/logdir/yourdomain-error.log"
    accesslog.filename = "/path/to/your/logdir/yourdomain-access.log"
    fastcgi.server = (
        "/youdomain.fcgi" => (
            "main" => (
                "host" => "127.0.0.1",
                    "port" => 3000,
            )
        ),
    )
    alias.url = (
        "/media" => "/usr/lib64/python2.5/site-packages/django/contrib/admin/media/",
        "/favicon.ico" => "/path/to/favicon/favicon.ico",
    )
    url.rewrite-once = (
        "^(/media.*)$" => "$1",
        "^/favicon.ico" => "/favicon.ico",
        "^(/.*)$" => "/yourdomain.fcgi$1",
    )
}
```

Some quick notes about this. The regex pattern at the top makes sure you can use both www.yourdomain.com and yourdomain.com to visit the site. The media alias url is used for the Django Admin site and it's media content. So change this path to wherever you have installed the Django source-code. The favicon part is optional. You only have to add that if you actually have a favicon and want to display it. As you can see, there's also another fast-cgi script mentioned in the configuration. We are going to create this file now. Open an editor and put the following line in it

```bash
#!/bin/sh
export DJANGOSETTINGSMODULE=yourproject.settings.main
```

Place this script in your Django project folder. Also make sure it is executable

```bash
chmod +x /path/to/your/docroot/yourdomain.fcgi
```

Now try to start lighttpd

```bash
/etc/init.d/lighttpd start
```

If this works, you can also add lighttpd to the default runlevel

```bash
rc-update add lighttpd default
```

Ok, now you should be able to visit yourdomain.com and see your Django application. When I first tried this, I ran into a small problem. Sometimes, the url looked weird because it displayed the name of the fast-cgi script in it. I was able to solve this issue by adding the following line to my `settings.py` file

```bash
FORCE_SCRIPT_NAME = ''
```

I hope I didn't miss anything, since it's already been a while when I set up this environment.

One last note, you could also run the Django fast-cgi process using Unix sockets rather than using a TCP connection. I didn't try it myself but it looks pretty easy. You can find more information about that [here](http://code.djangoproject.com/wiki/InitdScriptForLinux).

I hope this post helps some people to get things running correctly.
