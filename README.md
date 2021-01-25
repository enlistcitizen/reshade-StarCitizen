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

Once you clone this repo, Git for Windows shell extension will allow you to right click on the cloned repo folder and run a bash terminal.
Do that... then run the following two git commands in your repo cloned folder to pull the updates for the submodules:

```git
git submodule update --init --recursive
git submodule update --remote
```

Next to install is Visual Studio 2019 which is free to use and can be downloaded at [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com/)
During the install you will get a window asking if you want to install additional tools and features.
You should install Python Development, Windows Development, Python 3 64-bit and 32-bit, and Windows 10 SDK 10.0.17763.0

This specific Windows 10 SDK is required because of the parent repo.
After this is installed you will need two additional libraries.

Now that you have installed Python as a feature within Visual Studio, file explore will have a context right click menu to be able to open and run python files.
Go to your repo then the deps folder, then the gl3w folder. Right click gl3w_gen.py and select **Edit with IDE**. Press F5 to run this module.
This script will do the following
```python
Downloading include/GL/glcorearb.h...
Downloading include/KHR/khrplatform.h...
Parsing glcorearb.h header...
Generating include/GL/gl3w.h...
Generating src/gl3w.c...
```
Which you will need for successfully completing the build directions below.


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

## Contributing

Any contributions to the project are welcomed, it's recommended to use GitHub [pull requests](https://help.github.com/articles/using-pull-requests/).

## License

All source code in this repository is licensed under a [BSD 3-clause license](LICENSE.md).
