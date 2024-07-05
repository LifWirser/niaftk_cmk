# niaftk_cmk
creating "CmakeLists" for NAIF toolkit (currently 2 versions working through install "c &amp; fortran"

Instructions
choose branch for language desired and dl CmakeLists.txt
assuming you already have the appropiate source uncompressed put the "CmakeLists.txt in main directory
linux
mkdir biuld && cd build
(either cmake or ccmake) . {the period is important)
upon complete generation then either run "make" or "cmaqke --build ."

the "c" version seems to build and install (cookbook programs do not compile)
The fortran version has problems setting "fflags" so beware ccmake "t" option allows fflags to be change
 however cookbook examples do compile

 Also these two cmake files both modify the source tree they are started in, Be warned
 If you still have these source archive just hang on to it just in case.

 
