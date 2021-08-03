# Basic Template for wxWidgets on Windows 10 Visual Studio Code
Since I found this to be a bit of a nightmare, I thought I'd put down the process I went through to set up wxWidgets in VSCode.
I'll first go through setting up C++ and MinGW, then downloading and building wxWidgets and finally setting it up for VSCode.
There are a variety of sources for doing all this online and I'll link all the ones I used here.

## Setting up C++

[Using GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw)

First off I started here, the instructions are easy enough to follow.

1. Go to extensions in VSCode and search C++
2. Click the C/C++ extension by microsoft (should be the top one) and click install
3. Click the "Click here" on the link to download the MSYS2 installer

[MSYS2 Website](https://www.msys2.org/)

4. You are then instructed to go to the MSYS2 website (above link) for installing Mingw-w64
5. Run the installer you just downloaded
6. Specify your installation folder and press next till the installation is complete
7. Tick the box for opening MSYS2 64bit Wizard and click finish
8. When it opens first run ```pacman -Syu``` inputing ```Y``` whenever prompted
9. Once it's done it'll then close
10. Search MSYS from the start menu and open MSYS2 MSYS
11. Once it opens run ```pacman -Su``` entering ```Y``` when prompted
12. After this all done then run ```pacman -S --needed base-devel mingw-w64-x86_64-toolchain``` press enter to install all packages (this is one of the bits that tripped me up there's no probably about it you need to do this to actually install the compiler)

With that we're done with MSYS2, next you need to add the compiler path to the system environmental variables. They go through this on the VSCode website you were on before the MSYS2 website.

13. Search "environment" from the start menu and select "Edit the system environment variables" (should be at the top)
14. Click the "Environment Variables..." button (bottom right corner)
15. Find the "Path" variable (there should be two one under user variables and the other under system variables, I updated both), click on it then click "Edit..."
16. Now you want to find the ```bin``` folder for MinGW64 which you just installed and copy it (the path should look something like this ```C:\msys64\mingw64\bin``` where ```C:\msys64``` was my installation folder)
17. Back with editing the "Path" variable, click "New" and paste the path to the ```bin``` folder you just copied
18. Then click "OK" to finish up

The VSCode website then goes through setting up VSCode and checking the installation worked. I found it helpful but I'll cover all that later. Firstly, I'd restart your device to make sure everything updates correctly. I didn't and didn't realise there was an issue for quite a while since gFortran also has a g++ compiler (no, I didn't know initally and yes, this cleared up alot of my initial confusion as to why this was all necessary).

## Setting up wxWidget

So now for downloading and building wxWidgets. It mentions there are a few prebuilt binaries but it recommends building from source which is what I'll go through here.

[wxWidgets download page](https://www.wxwidgets.org/downloads/)

1. There are a few different releases shown on the download page (I downloaded the latest one 3.1.5), pick one and download the "Windows ZIP" file from the "Source Code" section
2. You then want to extract the ZIP to any directory (I created a ```C++\wxWidgets315``` folder in ```Documents```) this will possibly take a few minutes
3. You then want to create another enironment variable containing the full path to the directory you extracted the ZIP file to (follow point 13 and 14 from the previous section)
4. Under the systems variables section select "New..."
5. For the variable name input ```WXWIN``` and for the value copy in the full path to your wxWidgets directory. Then press "OK" to finish up.

[Building wxWidgets](https://wiki.wxwidgets.org/Compiling_wxWidgets_with_MinGW)

I then followed the instructions from here for the most part.

6. Search "cmd" from the start menu and open the "Command Prompt" (should be at the top)
7. ```cd``` into the directory you extracted the ZIP file to (```cd %WXWIN%``` should do it if the environment variable is set up properly).
8. Next ```cd .\build\msw```
9. Then to build the library you want to input ```mingw32-make -f makefile.gcc SHARED=1 UNICODE=1 BUILD=debug```, this will build a dynamic debug library.
10. If this works properly it should take ages. So now all you have to do is wait (or find somthing to do like have breakfast/lunch/dinner, go for a walk, read a book, clean a bathroom)

If there are any errors at this stage, try rebuilding or restarting your device then rebuilding. You can also clean it by appending ```clean``` to the command mentioned in this sections point 9 and then build again.

11. To build the release verion of the library just run ```mingw32-make -f makefile.gcc SHARED=1 UNICODE=1 BUILD=release``` where ```BUILD``` is changed from ```debug``` to ```release```

That should be everything for downloading and building wxWidgets from source. This should create a DLL under ```lib\gcc_dll``` in the your wxWidgets directory. Inside this folder you should also find an ```mswud``` folder for the debug version and an ```mswu``` folder for the release version (and then a load of other stuff).
