# Build mpv with libplacebo and vulkan support on macOS
Build and install `vulkan` and `libplacebo`.

Build mpv:
```
cd ~/Downloads/mpv
./bootstrap.py
CFLAGS="-I/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/include" LDFLAGS="-L/Users/anindya/Downloads/vulkansdk-macos-1.2.162.0/macOS/lib" ./waf configure --prefix='/Users/software'
./waf
```
