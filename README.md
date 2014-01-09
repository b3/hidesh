
Goals
=====

I give lectures and practical works on Unix and shell scripting to students as
part of my jobs. The way I do these practical works is that I give them some
kind of specifications and uses examples. Then I hope they will understand
what I want them to do and wait. They often do not get exactly what the script
have to do.

What could be helpful is to enable them to have access to a working version of
the script but without them to be able to have access to the source of it. It
is not possible to do that through simple Unix rights managements : in order
to execute a shell script one has to be able to read it.

Having a binary version of a shell script is why I wrote hidesh. This program
takes a shell script and write a simple C file which once compiled runs the
script. I can then distribute the produced binary to my students who can then
test the script without knowing how it is written. With a little work they
could be able to find the source code in memory. If they are able to do that
it's fine for me because they do not need my lessons anyway :-)

How it works
============

1- Read the shell script

2- Obfuscate it a little bit (base64 for instance)

3- Write a C file which embed that obfuscated version in a constant string and
   which :
   
   1- deobfuscate the string
   2- create a named pipe
   3- fork
   4- the child process then execv a shell
   5- the main process write the shell code to the named pipe
   6- the main process wait for its child to die
   7- the main process remove the named pipe
