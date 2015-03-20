---
layout: post
title:  "Speed up your tests (the easy one)"
date:   2014-12-18 
categories: devops
---
How many times do you run your tests and get stuck waiting points to appear in your shell?
Well, other than take a look in writing tests (I'll write an article about that, I promise), probably your cli-php is running on top of xdebug.
Xdebug do lots of things but, if your programming style is TDD-like, you can't wait too long. So let's take a look on a simple trick you can use for speeding up your tests execution.

First, you have to localize your php.ini file:

{% highlight bash %}
php -i | grep php.ini
{% endhighlight %}

The shell will output the path of your loaded php.ini file.
From this point in time, every step can change based on your machine and/or configuration, so adapt the steps to your case.

Looking in the php.ini file, you should find something similar to these lines

{% highlight bash %}
[xdebug]
zend_extension="/usr/local/Cellar/php55-xdebug/2.2.5/xdebug.so"
{% endhighlight %}

So you're loading xdebug in your cli, therefore in your phpunit, therefore in your tests.
What can you do in order to load xdebug whenever you want?
the solution is pretty simple.
Duplicate your php.ini file and name it php.ini.no-xdebug
Open the php.ini.no-xdebug file and delete the lines that loads xdebug ([xdebug] section).

Now, you've to add aliases to your shell.
Open your .bashrc (or .zshrc) and add these aliases:

{% highlight bash %}
alias phpunit="php -c /usr/local/etc/php/5.5/php.ini.no-xdebug bin/phpunit"
alias xphpunit="php -c /usr/local/etc/php/5.5/php.ini bin/phpunit"
{% endhighlight %}

At this point, you can run the `phpunit` command in order to run the tests without loading xdebug.
