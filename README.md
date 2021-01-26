ReShade
=======

This is a generic post-processing injector for games and video software. It exposes an automated way to access both frame color and depth information and a custom shader language called ReShade FX to write effects like ambient occlusion, depth of field, color correction and more which work everywhere.

The ReShade FX shader compiler contained in this repository is standalone, so can be integrated into other projects as well. Simply add all `source/effect_*.*` files to your project and use it similar to the [fxc example](tools/fxc.cpp).

## Fork Notes
detect high network traffic commented out so the directional depth of field filter and other creative depth filters can be used in Star Citizen alpha for creative screenshots and videos. Star Citizen is not a competitive FPS game with smoke screen bombs so enabling the ability to use depth of field filters should not be a cheating risk for Star Citizen; however, this repo should not be used for any malicious usage or intended usage to cheat or bypass any security measures of any program. Such malicious use is strictly forbidden. This repo fork is solely intended for creative content creation use only.

## Fork Amended Install Notes
If you are new to programing and Visual Studio like me, getting this working without more complete guidance is a nightmare.
First thing is first, make sure your Windows PC is git capable.
Download and install Git for Windows [https://gitforwindows.org/](https://gitforwindows.org/)

Now download and use your Git GUI of choice. I like GitHub Desktop from GitHub. This will be easier to use than manually using Git for Windows to clone this repo onto your computer. Once your favorite Git GUI is installed, clone this repo into a working folder near or at the root of your working storage drive. I don't use my operating system drive for anything other than operating system stuff. But if you have only one drive, you can also use a large and fast USB stick for everything we are about to do, if you wish.

Once you clone this repo, Git for Windows shell extension will allow you to right click on any git folder and run a bash terminal.
You can do the following for good measure. Go to the Repo you fetched, to do the DEP folder, right click open bash terminal here.
Then run the following git command in your repo DEP folder to pull the updates for the submodules:

```git
git submodule update --init --recursive
```

Next to install is Visual Studio 2019 which is free to use and can be downloaded at [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com/)
During the install you will get a window asking if you want to install additional tools and features.
You should install Python Development, Windows Development, Python 3 64-bit and 32-bit, and Windows 10 SDK 10.0.17763.0

This specific Windows 10 SDK is required because of the parent repo.

Now that you have installed Git for Windows, Visual Studio 2019 or newer, Windows 10 SDK 10.0.17763.0, Windows and Python development tools....
We have two things left to do before moving onto to the directions below to make your first build!

We are going to test if Python is installed and declared properly in Windows 10.
Open command prompt at type
```cmd
python
```
Does anything happen other than a not found warning? if so, you are good to move onto the next last step. If not, we have some work to do.

Now type in command prompt
```cmd
py
```
Does anything happen? Either way, we need to install Python for windows in your particular Windows enviornment.
Even though we already installed Python through Visual Studio 2019 and have the option to edit py files in an editor via right click content menu, we need Python to work with the command python within command prompt in order to have a successful build within Visual Studio 2019... in a dummy proof manner.
Go to [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) and download the most recent installer version of Python and make sure to match your Operating System's x32 or x64... most gaming PCs will be x64 bit.

Now install this newest Python version right next to your previous Python installation in `Microsoft Visual Studio\Shared\` or whever your previous Python was installed by Visual Studio. Visual Studio 2019 will not install the most recent Python for Windows builds and you can have more than one build on your system, our only goal here is to get the `python` command working. During the Python for Windows install it will present you some options. Enable Python for all users and enable the launcher option, the rest of the options are at your discretion. Once this install is complete, open command prompt and type `python`. if it prints out an option to type help, you have success! Now close command prompt and move onto the last step.

Now to get ready for your build. Launch Visual Studio 2019, it will ask you to open a project. Navigate to your forked repo and open the `ReShade.sln` file.
You will have an overwhelming interface at first if you are new to this. But if you have any experince making websites or using photoshop or after effects, you will be able to figure this out. Click over near the top right on your main Reshade Solution in that right window. Do not select the project files which are lower in that list. Right click your main ReShade Solution and select **Retarget**. A popup with give you the option for two things to do to all project files. Update the Platform Toolset and/or update the Windows SDK Version. Do not update the Windows SDK Version.. make sure in that drop down you select No Uprade. In the Platform Toolset make sure it selected to upgrade to your most recent version number (highest number) and make sure everything below is check marked. Click Ok.

You are now ready to start building!!! Make sure in the top middle of your Visual Studio screen that you select Release and 32-bit FIRST.. build it. If it fails, review your output log and fix whatever is wrong. Google is your friend. Then ReBuild, until you have a build with ZERO failures. Then select the 64-bit on the drop down and build. If your first build at 32-bit did not fail, you should be clear sailing all the way. Once both of those Release builds are done, you can then do `Release Setup` and 64-bit for your own EXE package. I don't take any responsibility if you screw things up, this is at your own risk and you should do some reading to be familiar with all I have shared. This is self help stuff, and this is what I discovered from my Journey. Enjoy and happy Star Citizen Screenshot making!


## Building

You'll need Visual Studio 2017 or higher to build ReShade and Python for the `gl3w` dependency.

1. Clone this repository including all Git submodules
2. Open the Visual Studio solution
3. Select either the `32-bit` or `64-bit` target platform and build the solution.\
   This will build ReShade and all dependencies. To build the setup tool, first build the `Release` configuration for both `32-bit` and `64-bit` targets and only afterwards build the `Release Setup` configuration (does not matter which target is selected then).

A quick overview of what some of the source code files contain:

|File                                                      |Description                                                            |
|----------------------------------------------------------|-----------------------------------------------------------------------|
|[dll_log.cpp](source/dll_log.cpp)                         |Simple file logger implementation                                      |
|[dll_main.cpp](source/dll_main.cpp)                       |Main entry point and test application when building for debug          |
|[dll_resources.cpp](source/dll_resources.cpp)             |Access to DLL resource data (e.g. built-in shaders)                    |
|[effect_lexer.cpp](source/effect_lexer.cpp)               |Lexical analyzer for C-like languages                                  |
|[effect_parser.cpp](source/effect_parser.cpp)             |Parser for the ReShade FX shader language                              |
|[effect_preprocessor.cpp](source/effect_preprocessor.cpp) |C-style preprocessor implementation                                    |
|[hook.cpp](source/hook.cpp)                               |Wrapper around MinHook which tracks associated function pointers       |
|[hook_manager.cpp](source/hook_manager.cpp)               |Automatic hook installation based on DLL exports                       |
|[input.cpp](source/input.cpp)                             |Keyboard and mouse input management and window message queue hooks     |
|[runtime.cpp](source/runtime.cpp)                         |Core ReShade runtime including effect and preset management            |
|[runtime_gui.cpp](source/runtime_gui.cpp)                 |Overlay GUI and everything related to that                             |
|[d3d9/runtime_d3d9.cpp](source/d3d9/runtime_d3d9.cpp)     |Effect runtime implementation for D3D9                                 |
|[d3d10/runtime_d3d10.cpp](source/d3d10/runtime_d3d10.cpp) |Effect runtime implementation for D3D10                                |
|[d3d11/runtime_d3d11.cpp](source/d3d11/runtime_d3d11.cpp) |Effect runtime implementation for D3D11                                |
|[d3d12/runtime_d3d12.cpp](source/d3d12/runtime_d3d12.cpp) |Effect runtime implementation for D3D12                                |
|[opengl/runtime_gl.cpp](source/opengl/runtime_gl.cpp)     |Effect runtime implementation for OpenGL                               |
|[vulkan/runtime_vk.cpp](source/vulkan/runtime_vk.cpp)     |Effect runtime implementation for Vulkan                               |

## ReShade For Star Citizen

Congradulations! You have successfully made your ReShade build. Now either install it {*did you do a virus check? if not do one on* [https://www.virustotal.com/gui/](https://www.virustotal.com/gui/)} or if you already installed the stock ReShade and just need the dll go to your `reshade-StarCitizen\bin\x64\Release` folder and copy `ReShade64.dll` over to your ReShade files in your Star Citizen Bin64 folder. Rename this copied `ReShade64.dll` file to `d3d11.dll` so that Star Citizen will load ReShade. A recent update to both Windows and ReShade made things funky to where even stock ReShade will not work unless you rename the dll and log file to d3d11

If you used your own Build EXE just rename the installed dll and log files to `d3d11` and you will be good to go!
Enjoy and happy Star Citizen Screenshot making!

## Contributing

Any contributions to the project are welcomed, it's recommended to use GitHub [pull requests](https://help.github.com/articles/using-pull-requests/).

## License

All source code in this repository is licensed under a [BSD 3-clause license](LICENSE.md).
