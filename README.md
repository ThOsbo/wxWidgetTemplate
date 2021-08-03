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
