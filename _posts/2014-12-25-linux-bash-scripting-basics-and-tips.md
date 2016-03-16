---
layout: post
title: Linux bash scripting basics and tips
date: 2014-12-25 18:35:00 +0800
author: Yuan Jiang
tags: linux bash
---

Linux bash scripting basics and tips.

Shebang
{% highlight bash %}
$ #!/usr/bin/env bash => better portability
or
$ #!/bin/bash
{% endhighlight %}

Comments and quotes
{% highlight bash %}
$ # This is a comment line
$ echo '`date`' => print `date` literally
$ echo "`date`" => print actually date string
{% endhighlight %}

Debugging
{% highlight bash %}
$ #!/usr/bin/env bash -x
#or
$ #!/bin/bash -x
#or
$ bash -x script.sh
#or within script
$ set -x => start debugging
$ set +x => stop debugging
{% endhighlight %}

Variables
{% highlight bash %}
$ printenv # display global env
#or
$ env # display global env
$ set # display local env
$ var=value # define local var in current shell
# notice there is no space between the equal sign
$ export var=value # export var and available in sub shell
# special variables
$ $? # Exit status of most recently executed command
$ $0 # Current script name
$ $# # Number of parameters
$ $@ # Positional parameters array
$ $x # Positional parameter: x = 1, 2, 3, ...
{% endhighlight %}

Execute script in current shell
{% highlight bash %}
$ source script.sh
#or
$ . script.sh # notice the dot (.) command
{% endhighlight %}

Shell expansion
{% highlight bash %}
$ echo sp{el,il,al}l # spell spill spall
$ ~ # $HOME

$ $VAR # get var value
#or
$ ${VAR} # get var value

$ ${#var} # get length of var

$ $(command) # get command output
#or
$ `command` # get command output

$ $((expression)) # get arithmetic expression value
#or
$ $[expression] # get arithmetic expression value
#or
$ let "expression" # do arithmetic evaluation on expression
{% endhighlight %}

If condition
{% highlight bash %}
if [ condition1 ]
then
  command(s)...
elif [ condition2 ]
then
  command(s)...
else
  command(s)...
fi

where condition can be the following:
1) test => builtin
2) /usr/bin/test
3) [ ]
4) [[ ]]
5) /usr/bin/[
6) command => omit the [ or [[
7) (( )) => arithmetic
{% endhighlight %}

File test operators
{% highlight bash %}
-e => file exists
-a => same as -e but deprecated
-f => regular file, not directory or device
-s => file not zero size
-d => file is directory
-x => file has execute permission
-r => file has read permission
-w => file has write permission
-nt => newer than
-ot => older than
and etc.
{% endhighlight %}

Integer comparison operators
{% highlight bash %}
-eq => equal to
-ne => not equal to
-gt => greater than
-ge => greater than or equal to
-lt => less than
-le => less than  or equal to
Below need to be in double parenthesis:
<   => less than
<=  => less than or equal to
>   => greater than
>=  => greater than or equal to
{% endhighlight %}

String comparison operators
{% highlight bash %}
=  => equal to (whitespace around)
== => equal to
!= => not equal to
<  => less than
>  => greater than
-z => is null, zero length
-n => is not null
{% endhighlight %}

Compound comparison operators
{% highlight bash %}
-a  => logical and (same as && within double brackets)
-o  => logical or (same as || within double brackets)
!   => NOT
{% endhighlight %}

Loops - for loop
{% highlight bash %}
=> regular format:
for arg in [list]
do
  command(s)...
done

=> c-style for loop:
for ((i=1; i<=LIMIT; i++)); do
  command(s)...
done

=> without do/done:
for ((n=1; n<=10; n++))
{
  command(s)...
}
{% endhighlight %}

Loops - while/until loop
{% highlight bash %}
while [ condition ]; do
  command(s)...
done

until [ condition is true ]
do
  command(s)...
done

=> condition can be following formats:
1) [ condition ]
2) [[ condition ]]
3) (( condition ))
4) condition
5) command
6) multiple commands
7) function call
{% endhighlight %}

Branching - case/select
{% highlight bash %}
case "$var" in
  "$condition1")
  command...
  ;;
  "$condition2")
  command...
  ;;
  ...
esac

select var [in list]
do
  command...
  break
done

=> when [in list] is omitted, select list is the command line arguments ($@) passed to script or function containing the select.
{% endhighlight %}



## Reference links
- [BASH Programming - Introduction HOW-TO
](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)
- [Bash Guide for Beginners
](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/index.html)
- [Advanced Bash Scripting](http://www.tldp.org/LDP/abs/abs-guide.pdf)
- [Advanced Bash Scripting HTML Version](http://www.tldp.org/LDP/abs/html/)
