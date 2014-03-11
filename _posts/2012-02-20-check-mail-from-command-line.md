---
layout: post
title: "Check mail from command line"
description: ""
category: bash 
tags: []
---
{% include JB/setup %}

I’m not using an email client to check my mail for quite some time mainly because I don’t like being interrupted while I’m in the middle of something. Since I’m developing from a command line (guake), for convenience, I would like to check my mail without changing context so I found [this](http://www.commandlinefu.com/commands/view/3386/check-your-unread-gmail-from-the-command-line):

{% highlight ruby %}
  curl -u username --silent "https://mail.google.com/mail/feed/atom" | perl -ne 'print "\t" if //; print "$2\n" if /<(title|name)>(.*)<\/\1>/
{% endhighlight %}

You can use username:password if you’re comfortable with storing your password in plain text.

I’ve placed the above command in a script file added executable rights (+x) to that and I’m using it like: gmail my_account to check for incoming new mail.

Since I’m using multiple accounts, I’ve modified the script file to include the “all” command which will check all my email accounts. Currently you’ll have to introduce your password for each account. I’ll change this in a future post so that you can “safely” store you passwords and have a master password for checking all your accounts.

Here’s [the script for checking multiple gmail accounts](https://gist.github.com/1868462) by issuing the command: gmail all
