conditional-highlighter
=======================

highlight tables like excel's conditional formatting

Prior Art
---------
I found a few bits of code on the internet that do what this tries to
accomplish, but unfortunately for me were not suitable for the task.
The Perl module
[`Text::ANSITable`](https://metacpan.org/pod/Text::ANSITable) seemed the
most complete but wouldn't install on OSX as it lacks a proc filesystem.
There is also
[`ANSI::Heatmap`](http://search.cpan.org/~rjh/ANSI-Heatmap-0.3/README.pod)
which only accepts integer values.  There is a c implementation on
github called
[`terminal-heatmap`](https://github.com/jclulow/terminal-heatmap) that
segfaulted on my Mac when I tried to run it.  And so I was left with
writing my own implementation.

Dependancies
------------
###Tree::Interval
I've found this a little tricky to install as it shows up on CPAN when
doing a web search, BUT cannot be found when trying to install through
the command-line CPAN command.  I'm not sure if this is just a problem
with me or something more general.  If you can't install it, it is easy
enough to download the code manually from the website and install it on
your machine

###Convert::Color, Convert::Color::RGB8, Convert::Color::XTerm
These should all install fine through the cpan command-line interface

Caveats
-------
Currently `conditional_highlighter` will only work on terminals that
have 256 colors enabled.  To check that you do, type `tput colors` at
the prompt, if the result is 256 then your good to go!

Usage
-----
Without options `conditional_highlighter` will take tabular data and
highlight all numbers in a linear scale from blue-green to red. As an
example:
```bash
$ awk 'func r(){
  return sqrt(-2*log(rand()))*cos(6.2831853*rand())
}
BEGIN{
  for(i=0;i<28;i++)s=s"\n"0.5*r();
print s}' |\
paste - - - |\
sed 1d |\
conditional_highlighter
```
Anything that does not look like a number (acording to perl) will not be
processed, so a table with text headers or row labels won't affect
anything.  However if your labels are numbers then you will need to
explicetly remove the row or column from the comparison.  For example:
```bash
# bad highlighting!!
$ awk 'func r(){
  return sqrt(-2*log(rand()))*cos(6.2831853*rand())
}
BEGIN{
  for(i=0;i<28;i++){\
    j=0.5*r();
    if(i %3 == 0){
      j+=100;
    }
    print j\
  }\
}' |\
paste - - - |\
conditional_highlighter

# skip the first column!
$ awk 'func r(){
  return sqrt(-2*log(rand()))*cos(6.2831853*rand())
}
BEGIN{
  for(i=0;i<28;i++){\
    j=0.5*r();
    if(i %3 == 0){
      j+=100;
    }
    print j\
  }\
}' |\
paste - - - |\
conditional_highlighter -c 1
```
Use the `-c` and `-r` options to skip any rows and columns.  The
arguments take on the same format as the `cut` command so writing
`-c 1,3,6-10` will skip columns 1,3,6,7,8,9,10! Remember that the
arguments are 1-indexed.

Finally you can use the `-v` option to color the backgroud rather than
the foreground color.
