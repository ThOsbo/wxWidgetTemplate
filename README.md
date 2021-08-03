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

That should be everything for downloading and building wxWidgets from source. This should create a DLL under ```lib\gcc_dll``` in the your wxWidgets directory. Inside this folder you should also find an ```mswud``` folder for the debug version and an ```mswu``` folder for the release version (and then a load of other stuff). Another [wxWidgets site](https://docs.wxwidgets.org/trunk/plat_msw_install.html) which also goes through different ways to build the library and lists all the different makefile parameters you can add. The method used here is under the "Using makefiles from Windows command line" section.

## Setting up VSCode

So I briefly mentioned earlier that the VSCode website goes through some of this but I'll go through it all here. There was also a very useful video I found for this will I'll link in.

[Setup a basic wxWidgets 3.1.3 project with VS Code and MinGW](https://www.youtube.com/watch?v=tHMGA0jIl3Y)

This video was really helpful for setting things up. It goes through everything I mentioned here but uses CMake to build the wxWidgets libraries instead of the command line. I've set up this repository in almost exactly the same way they do in the video (minus the build folder containing the ```.dll``` files). I had a few warnings still upon finishing but I'll go through how I cleared them up here.

### Setting up tasks.json

1. Go to the "Terminal" tab at the top and click it, then go to the bottom of the drop down and click "Configure Default Build Task"
2. From the drop down from the command palette click on "C/C++:g++.exe build active file" (if there are multiple, select the one which states the compiler is ```mingw64\bin\g++.exe```), this will create a "tasks.JSON" file

If nothing is showing up or you can't find the correct one, create a ```.cpp``` file and repeat points 1 and 2 from this section.

3. In the "tasks.json" file, change the label to ```Debug```
4. Then in ```args``` under the ```-g``` flag change to ```${workspaceFolder}\\src\\*.cpp```
5. Next under the ```-o``` change the output file to ```${workspaceFolder}\\build\\debug\\${fileBasenameNoExtension}.exe```
6. And now to add a variety of stuff
   - Add ```-I``` flag and then ```${workspaceFolder}\\include``` under it 
   - Add ```-I``` flag and then ```${WXWIN}\\include``` under it
   - Add ```-I``` flag and then ```${WXWIN}\\lib\\gcc_dll\\mswud``` under it
   - Then add an ```-L``` flag and ```${WXWIN}\\lib\\gcc_dll``` under it
7. Now you want to go into the directory containing your wxWidgets build and go to ```\lib\gcc_dll``` in file explorer
8. Then you want to search "\*core" and find a file named something like ```libwxmsw31ud_core.a```. This is the debug version
9. Go back to "tasks.JSON" and add the ```-l``` flag, then under it copy the file name you just found and paste it in removing the extension and ```lib``` at the front (so it looks like ```wxmsw31ud_core```)
10. While you're there copy the coresponding ```.dll``` file (```wxmsw315ud_core_gcc_custom.dll```) and paste it into your ```build\debug``` folder in your VSCode workspace
11. Repeat points 7 to 10 but instead search for "\*base" (your looking for a file called ```libwxbase31ud.a``` and ```.dll``` file ```wxbase315ud_gcc_custom.dll```)

That should be all for setting up "tasks.json" for the debug verion. If you have the release version copy the entire thing starting from the ```{``` before ```type``` to the ```}``` after ```detail``` and paste it in ```tasks``` after the debug version you just edited. Then change the label from ```Debug``` to ```Release``` and just remove the ```d``` in ```wxmsw31ud_core```, ```wxbase31ud``` and ```${WXWIN}\\lib\\gcc_dll\\mswud```. Also change the output directory from ```${workspaceFolder}\\build\\debug\\${fileBasenameNoExtension}.exe``` to ```${workspaceFolder}\\build\\release\\${fileBasenameNoExtension}.exe``` and remove the ```-g``` flag at the beginning.

### Setting up c_cpp_properties.json

12. Press ```Ctrl + Shift + P``` to open up the command palette and type in "C/C++" and find and click on "C/C++: Edit Configurations (UI)"
13. Change the "Configuration name" to something like "GCC" 
14. Go down to the "Compiler path" section and click the drop down arrow
15. Scroll down and select ```mingw64\bin\g++.exe``` compiler
16. Then go down to "IntelliSense mode", click the drop down and select "windows-gcc-x64" (or whatever is appropriate)
17. Next, under the "Include path" section change ```${workspaceFolder}/**``` to ```${workspaceFolder}\include/**```
18. Then add (on seperate lines) ```${WXWIN}\include/**``` and ```${WXWIN}\lib\gcc_dll/**```
19. Finally, under the "Defines" section, add (on seperate lines) ```WXUSINGDLL``` and ```GCCVERSION``` (this will help fix a warning about not finding the correct source file later)

At this point everything should work fine although a warning along the line of "can't find source ..\..\..\lib\vc_dll\wx\setup.h" might be showing up. "vc" is the default prefix used for the library, everything should however build and run fine if you have completed the above. 
