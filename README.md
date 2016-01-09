#SuperNET Client "iguana"

[![Join the chat at https://gitter.im/jl777/SuperNET](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/jl777/SuperNET?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> #TL;DR#
> 
> ```sudo apt-get update; sudo apt-get install libcurl4-gnutls-dev libssl-dev; git clone https://github.com/jl777/SuperNET; cd SuperNET; ./m_onetime m_unix; ./m_unix; agents/iguana```
> 
> The above one line gets SuperNET installed, built and launched for unix. 
> 
> After that ```./m_unix``` updates to latest.
> *Continue below at "Running".*

**iguana is easy to build. Start by cloning (or downloading) this repository.**

#DEPENDENCIES#
##for native (unix, osx)##
Just make sure you have the dev versions of openssl and curl installed:

```sudo apt-get install libcurl4-gnutls-dev libssl-dev```

##For native (win32, win64)##
TOOL_DIR := /usr/local/gcc-4.8.0-qt-4.8.4-for-mingw32/win32-gcc/bin
MINGW := i586-mingw32
The above two definitions need to be changed to match the mingw install on your system. m_win32 and m_win64 just invokes the makefile in mingw32 and mingw64

##For chrome app##
You need to make sure the nacl sdk is properly installed and you are able to build the examples.
Now you will need to get the external libs, which can be built from scratch using naclports or there use the reference builds of libssl.a, libcrypto.a, libcurl.a and libz.a in the SuperNET/libs. You can just copy those over into $(NACL_SDK_ROOT)/lib/pnacl.


#ONETIME#
Now you are ready to build.
I try to make the build process as simple as possible, so there are no `autoconf`, `autoreconf`, `configure`, `cmake`, `make`, to get properly installed and running and run, etc. You do need a C compiler, like gcc.

The **first time** you need to build libcrypto777.a and to do that you need to run:
For unix: ```./m_onetime m_unix```
For osx: ```./m_onetime m_osx```
For win32: ```./m_onetime m_win32```
For win64: ```./m_onetime m_win64```

#(RE)BUILD
Once libcrypto777.a is built, you can build the agents.
For pnacl: ```./m_pnacl```
For unix: ```./m_unix```
For osx: ```./m_osx```
For win32: ```./m_win32```
For win64: ```./m_win64```

The m_(OS) is a standard I follow and should be self explanatory. within each is usually just a few lines, ie compile all the .c files and link with the standard libs.

To build just iguana, you can ```cd``` into SuperNET/iguana and do ```./m_unix``` (or ```./m_osx```, ...).

```./m_clean``` will remove the files created from the building

#RUNNING#
The native versions are command line applications: agents/iguana {JSON}
The chrome app pexe requires that the chrome is launched with a command line parameter (tools/chrome.localhost) and then browse to *http://127.0.0.1:7777* to see the pexe

#SUPERUGLYGUI#
Once iguana is running, you can see the superuglyGUI at *http://127.0.0.1:7778/?method*
by submitting API calls using the forms, you will see it go to some specific URL. You can also do a programmatic GET request to ```http://127.0.0.1:7778/api/<path to apicall>```

*http://127.0.0.1:7778/ramchain/block/height/0* -> full webpage

*http://127.0.0.1:7778/json/ramchain/block/height/0* -> JSON only

```curl --url "http://127.0.0.1:7778/ramchain/BTCD/block/height/0"``` --> full webpage returned (probably not what you want)
```curl --url "http://127.0.0.1:7778/json/ramchain/BTCD/block/height/0"``` --> returns just the json object from the api call

Internall, all paths convert the request into a standard SuperNET JSON request. you can use a POST command to directly submit such JSON requests:
```curl --url "http://127.0.0.1:7778/?" --data "{\"agent\":\"ramchain\",\"method\":\"block\",\"coin\":\"BTCD\",\"height\":0}"```
