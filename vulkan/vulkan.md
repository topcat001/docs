# Install and configure Vulkan SDK on macOS
Download the installation package from the link provided here:
https://vulkan.lunarg.com/doc/sdk/1.2.162.0/mac/getting_started.html.
To install, simply copy the contensts of the .dmg to the desired location:
`/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/`

Next, the ICD and layer files need to be copied into the etc directory:
```
mkdir -p /Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/etc/vulkan/
cp -rv /Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/share/vulkan/icd.d /Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/etc/vulkan/
cp -rv /Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/share/vulkan/explicit_layer.d /Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/etc/vulkan
```

Next, the pkg-config files need to be set up properly in
`vulkansdk-macos-1.2.162.0/macOS/lib/pkgconfig/`:

vulkan.pc:
```
prefix=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS
libdir=${prefix}/lib
includedir=${prefix}/include

Name: vulkan
Description: Vulkan
Version: 1.2.162.0
Libs: -L${libdir} -lMoltenVK
Libs.private: -lm
Cflags: -I${includedir}
```

SPIRV-Tools-shared.pc:
```
prefix=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS
exec_prefix=${prefix}
libdir=${prefix}/lib
includedir=${prefix}/include

Name: SPIRV-Tools
Description: Tools for SPIR-V
Version: 2020.7.0
URL: https://github.com/KhronosGroup/SPIRV-Tools

Libs: -L${libdir} -lSPIRV-Tools-shared
Cflags: -I${includedir}
```

SPIRV-Tools.pc:
```
prefix=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS
exec_prefix=${prefix}
libdir=${prefix}/lib
includedir=${prefix}/include

Name: SPIRV-Tools
Description: Tools for SPIR-V
Version: 2020.7.0
URL: https://github.com/KhronosGroup/SPIRV-Tools

Libs: -L${libdir} -lSPIRV-Tools-opt -lSPIRV-Tools -lSPIRV-Tools-link
Cflags: -I${includedir}
```

spirv-cross-c-shared.pc:
```
prefix=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS
exec_prefix=${prefix}
libdir=${prefix}/lib
sharedlibdir=${prefix}/lib
includedir=${prefix}/include/spirv_cross

Name: spirv-cross-c-shared
Description: C API for SPIRV-Cross
Version: 0.44.0

Requires:
Libs: -L${libdir} -L${sharedlibdir} -lspirv-cross-c-shared
Cflags: -I${includedir}
```

These can be checked with:
`pkg-config --modversion name`
where `name` is the file name (without .pc).

The shell environment needs to be set up properly so that the libraries are
found:

Excerpt from .profile:
```
# For Vulkan.
export VULKAN_SDK=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS
export PATH=$VULKAN_SDK/bin:$PATH
export DYLD_LIBRARY_PATH=$VULKAN_SDK/lib:$DYLD_LIBRARY_PATH
export VK_ICD_FILENAMES=$VULKAN_SDK/etc/vulkan/icd.d/MoltenVK_icd.json
export VK_LAYER_PATH=$VULKAN_SDK/etc/vulkan/explicit_layer.d
export PKG_CONFIG_PATH=$VULKAN_SDK/lib/pkgconfig:$PKG_CONFIG_PATH
```

Please refer to the getting started guide linked above for testing.
