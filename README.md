conditional-highlighter
=======================

highlight tables like excel's conditional formatting

Dependancies
------------
###Tree::Interval
I've found this a little tricky to install as it shows up on CPAN when
doing a web search, BUT cannot be found when trying to install through
the command-line CPAN command.  I'm not sure if this is just a problem
with me or something more general.  If you can't install it, it is easy
enough to download the code manually from the website and install it on
your machine

###Color::Convert, Color::Convert::RGB8, Color::Convert::XTerm
These should all install fine through the cpan command-line interface

Caveats
-------
Currently `conditional_highlighter` will only work on terminals that
have 256 colors enabled.  To check that you do, type `tput colors` at
the prompt, if the result is 256 then your good to go!

Usage
-----
Without options `conditional_highlighter` will take tabular data and
highlight all numbers in a linear scale from blue-green to red.
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
