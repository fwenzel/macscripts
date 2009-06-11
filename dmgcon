#!/bin/sh
#
# dmgcon
#
# version 2.0, 2003-03-25
#
# Shrink Mac OS X disk images to minimum size and convert to UDZO
# (read-only compressed), Mac OS X 10.1 or later.
#
# To use tcsh shell autocompletion with this script, add the
# following line to your completions.mine file:
# complete dmgcon 'p/*/f:*.{dmg,img}/'
# e.g.:
# echo "complete dmgcon 'p/*/f:*.{dmg,img}/'" >> ~/Library/init/tcsh/completions.mine
#
# This 'sh' shell script is public domain.
#
# Last modified:
# 2003-03-25 Carsten Klapp <carstenklapp@yahoo.ca>
#


##
# Usage
##

if [ $# -lt 1 -o $# -gt 2 ]; then
   echo "Usage: `basename $0` file.dmg" 1>&2
   exit 1
fi


##
# Settings & temporary filenames
##

SETFILE="/Developer/Tools/SetFile"
ORIGINALFILE="$1";
TEMPFILE="${ORIGINALFILE}.temp-shadow.dmg"
COMPRESSEDFILE="${ORIGINALFILE}.compressed.dmg"
BACKUPFILE="${ORIGINALFILE}.bak"


##
# Sanity checks
##

# input file exists?
if [ ! -f "${ORIGINALFILE}" ]; then
   echo "Sorry, file '${ORIGINALFILE}' not found." 1>&2
   exit 1
fi

# don't overwrite any of our previous temporary files
errormsg="Sorry, %s '%s' is already present.\nDelete it if you wish to try again.\n"

if [ -f "${TEMPFILE}" ]; then
   printf "${errormsg}", "a temporary file", "${TEMPFILE}"
   exit 1
fi

if [ -f "${COMPRESSEDFILE}" ]; then
   printf "${errormsg}", "a temporary file", "${COMPRESSEDFILE}"
   exit 1
fi

if [ -f "${BACKUPFILE}" ]; then
   printf "${errormsg}", "a backup file", "${BACKUPFILE}"
   exit 1
fi


##
# Main
##

# shrink disk image to the minimum possible size, and use a shadow
# file to save disk space
hdiutil resize -size min "${ORIGINALFILE}" -shadow "${TEMPFILE}"

# convert the disk image into a compressed disk image, but use the
# shrunken shadow file
hdiutil convert -imagekey zlib-level=9 -format UDZO "${ORIGINALFILE}" -shadow "${TEMPFILE}" -o "${COMPRESSEDFILE}"


##
# Cleanup
##

# junk the temp file if it exists
[ -f "${TEMPFILE}" ] && rm "${TEMPFILE}"

# rename the old file if conversion was successful
if [ -f "${COMPRESSEDFILE}" ]; then
   mv "${ORIGINALFILE}" "${BACKUPFILE}"
   mv "${COMPRESSEDFILE}" "${ORIGINALFILE}"

   ##
   # Bonus: set type and creator if developer tools are present
   ##
   if [ -f "${SETFILE}" ]; then
      "${SETFILE}" -t devr -c ddsk "${ORIGINALFILE}"
      "${SETFILE}" -t devr -c ddsk "${BACKUPFILE}"
   fi

   ##
   # Output: display the old and new file sizes
   ##
   ls -alF "${BACKUPFILE}" "${ORIGINALFILE}"
else
   # something went wrong
   echo "Sorry, the disk image '${ORIGINALFILE}' could not be converted."
   echo "Please review the above output for errors."
fi

##
# Done
##
