---
layout: post
title:  "gEdit External Tool For Compiling And Running A Single C++ File"
date:   2013-10-01 01:15:00
tags: code editor
---

###Run and compile C++ sources with a shortcut.

Today I made a gEdit external tools plugin for myself (and fellow gEdit users, of course) which compiles a single `.cpp` file and then runs it in the terminal using bash script. There is a built in feature in gedit which lets user create their own scripts like this, which uses shell to run them with given parameters from gedit.

<!--more-->

The script goes like this-

```bash
 #!/bin/sh
 g++ ${GEDIT_CURRENT_DOCUMENT_NAME%} -o ${GEDIT_CURRENT_DOCUMENT_NAME%.*}
 if [ -f ${GEDIT_CURRENT_DOCUMENT_NAME%.*} ];
 then
     gnome-terminal –working-directory=$GEDIT_CURRENT_DOCUMENT_DIR -e "bash -c \"./${GEDIT_CURRENT_DOCUMENT_NAME%.*}; read line;\"" &
 fi
```

You can also find it here <https://gist.github.com/mathbender/6768896>

The basic idea behind this is simple, take a file, compile it through g++ compiler (there should be a fallback version for gcc, but it isn't here now), and then, if compilation complete, run this file through a terminal. I also added a `read line;` command, so that **it waits for an enter before it exits**.

The way to use it in your own gEdit- install external tools first (use `sudo apt-get gedit-plugins` if necessary), then activate external tools from edit>preferences>plugins, and then go to tools>manage external tools and add a new one. In the text box, paste this code (and assign it a shortcut if you like) to use it directly in your code.

I also made a snippet for quickly inserting all the regularly needed headers in my cpp file, I think I'll be sharing that if it seems necessary.

Don't hesitate to tell me what you think! Till then, good night.<br/>Mahi