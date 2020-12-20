# Install libplacebo with vulkan support on macOS
Refer to vulkan build notes under the `vulkan` folder for vulkan setup.
Dependencies installed: `cmake`, `libepoxy` from homebrew, and `mako` through `pip3`:
`pip3 install mako`

Set up `meson` build configuration:
```
cd ~/Downloads/libplacebo
DIR=./build
CFLAFS="-I/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/include" CPPFLAGS="-I/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/include" LDFLAGS="-L/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/lib" meson $DIR
meson --reconfigure $DIR -Dprefix=/Users/software -Dvulkan-registry=/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/share/vulkan/registry/vk.xml
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
