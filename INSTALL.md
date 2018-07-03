# Install aRPGtool 

## Prerequisites

- To use the json functions you need the service programs YAJL & YAJLR4 from [Scott Klement](https://www.scottklement.com/yajl/ "Scott Klement") site.
- Optional, your can install [RPGUnit](http://rpgunit.sourceforge.net/ "RPGUnit") to test the installation.

## Install
Create the physical source files on your IBM i (or modify and use the script putarpg.in).

    CRTSRCPF FILE(YOURLIB/ARPG_BUILD) RCDLEN(92)  TEXT('aRPGtool - Build sources')
    CRTSRCPF FILE(YOURLIB/ARPG_CLLE)  RCDLEN(92)  TEXT('aRPGtool - CL sources')
    CRTSRCPF FILE(YOURLIB/ARPG_CMD)   RCDLEN(92)  TEXT('aRPGtool - Command sources')
    CRTSRCPF FILE(YOURLIB/ARPG_DDS)   RCDLEN(92)  TEXT('aRPGtool - File sources')
    CRTSRCPF FILE(YOURLIB/ARPG_H)     RCDLEN(112) TEXT('aRPGtool - Prototypes')
    CRTSRCPF FILE(YOURLIB/ARPG_HTML)  RCDLEN(212) TEXT('aRPGtool - HTML templates')
    CRTSRCPF FILE(YOURLIB/ARPG_LNK)   RCDLEN(92)  TEXT('aRPGtool - Exports')
    CRTSRCPF FILE(YOURLIB/ARPG_RPGLE) RCDLEN(112) TEXT('aRPGtool - Program sources')
    CRTSRCPF FILE(YOURLIB/ARPG_TEST)  RCDLEN(112) TEXT('aRPGtool - RPGUnit test cases')
    CRTSRCPF FILE(YOURLIB/ARPG_UIM)   RCDLEN(92)  TEXT('aRPGtool - Command help')
    CRTPF    FILE(YOURLIB/ARPG_META)  RCDLEN(80)  TEXT('aRPGtool - Meta data')

Upload the source code

    > cd /download_path_arpgtool/src
    > ftp your.host
    ftp> ascii
    ftp> cd /qsys.lib/yourlib.lib
    ftp> mput ARPG*.FILE/*.MBR
    ftp> bye

Compile the main build CLs 

    > ADDLIBLE LIB(YourSrcLib) or CHGCURLIB CURLIB(YourSrcLib)
    > CRTBNDCL PGM(YourSrcLib/B_ALL_DBG) SRCFILE(YourSrcLib/ARPG_BUILD) SRCMBR(B_ALL_DBG)
    > CRTBNDCL PGM(YourSrcLib/B_ALL_DIS) SRCFILE(YourSrcLib/ARPG_BUILD) SRCMBR(B_ALL_DIS)

*  B\_ALL\_DBG creates all objects with debug informations without optimization.
*  B\_ALL\_DIS removes all debug informations and does a full optimization.

Run the compilation :

    > CALL PGM(B_ALL_DBG) 
    or
    > CALL PGM(B_ALL_DBG) PARM(YourObjLib)

Optional, as first parameter you can set a different creation library.

If you have [RPGUnit](http://rpgunit.sourceforge.net/ "RPGUnit") in your library list, the CL will also create and execute the unit tests.


       
