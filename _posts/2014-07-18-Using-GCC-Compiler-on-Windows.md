---
layout: post
title:  "Using GCC Compiler on Windows"
date:   2014-07-18
tags:
  - C
  - C++
  - gcc
  - windows
---

Being a student of an Indian college I got to learn C and C++ in starting of college using old and ancient TC compiler(on windows) which is not as per current standards that is C11 standards, as the wikipedia page of [C programming language][1] says that current standard of C compiler is defined by [C11][2]. When I came across experienced programmers they opened my eyes by telling same fact; so I decided to make my hand smooth in gcc and g++ compilers too which are as per C11 standards (have a look at [list of C/C++ compilers][3]). In Linux using these compilers is not at all a pain but recently I came across a situation when I have to run few C programs on Windows and I wanted to use only C11 standard compiler.

When I installed gcc on windows, realized that it could not be easy for newbies; so here presenting few steps to be followed to install and configure gcc and g++ compilers on Windows:

1. Download and Install [MinGW][4] from [sourceforge][5]. *It is a Minimalist GNU for Windows; basically it allows us to run gcc([GNU Compiler Collection][6]) and GNU builtins on windows*.  
2. After installion in the end it will ask to add some packages, we need at least GNU C++ compiler. Mark it for installation and other packages if you need.  
![mingw1 image]({{ site.url }}/img/posts/mingw1.jpg)
3. Click on installion menu above ---> Apply changes ---> Apply. While installing it will look something like below:   
![ming2 image]({{ site.url }}/img/posts/ming2.jpg)

4. Finally after installion it will look something like below image with slowing installed version. Close this window  
![ming3 image]({{ site.url }}/img/posts/ming3.jpg)

5. Set its bin path(mostly it is C:\MinGW\bin) to environment variable. For this follow following steps:
    > Right click on my computer ---> Properties ---> Advanced ---> Environment Variables ---> system variables search for path variable ---> edit path ---> put a semicolom(;) at end of current path and append required path.  

Now to run your C programs:  
* Open command prompt  
* Move to the folder where your .c or .cpp files reaside which you want to run. On command prompt write:  
`gcc <filename>.c -o <filename>.exe`  and hit enter  
`<filename>.exe` hit enter  

You will see result on command prompt.

That's it, hopefully this would be helpful for you, in case of any problem you are welcome to put them in comments.


[1]: https://en.wikipedia.org/wiki/C_%28programming_language%29
[2]: http://en.wikipedia.org/wiki/C11_%28C_standard_revision%29
[3]: http://en.wikipedia.org/wiki/List_of_compilers#C.2B.2B_compilers
[4]: http://www.mingw.org/
[5]: http://sourceforge.net/projects/mingw/files/latest/download
[6]: http://en.wikipedia.org/wiki/GNU_Compiler_Collection