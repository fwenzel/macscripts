dmgcon
======

version 2.0, 2003-03-25
-----------------------

Shrink Mac OS X disk images to minimum size.
dmg images are also converted to UDZO (read-only compressed).
Requires Mac OS X 10.1 or later.

To use tcsh shell autocompletion with this script, add the
following line to your completions.mine file:
complete dmgcon 'p/*/f:*.{dmg,img}/'
e.g.:
echo "complete dmgcon 'p/*/f:*.{dmg,img}/'" >> ~/Library/init/tcsh/completions.mine

This 'sh' shell script is public domain.

Last modified:
--------------
2009-06-11 Frederic Wenzel <fwenzel@mozilla.com>
2003-03-25 Carsten Klapp <carstenklapp@yahoo.ca>


