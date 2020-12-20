# Install libplacebo with vulkan support on macOS
Refer to vulkan build notes under the `vulkan` folder for vulkan setup.
Dependencies installed: `cmake`, `libepoxy` from homebrew.

Set up configuration:
```
DIR=./build
meson $DIR
meson --reconfigure $DIR -Dvulkan-registry=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/share/vulkan/registry/vk.xml
meson --reconfigure $DIR -Dprefix=/Users/software
```

Verify, build, and install:
```
meson configure $DIR
ninja -C$DIR
ninja -Cbuild install
```

Shell environment setup in .profile:
```
# For stuff installed in /Users/software.
export DYLD_LIBRARY_PATH=/Users/software/lib:$DYLD_LIBRARY_PATH
export PKG_CONFIG_PATH=/Users/software/lib/pkgconfig:$PKG_CONFIG_PATH
```
