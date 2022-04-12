# ALP flycast

How to build standalone flycast (reicast.elf)

- source: https://github.com/flyinghead/flycast
- build step
```	
# prepare
git clone https://github.com/flyinghead/flycast.git
cd flycast
git submodule update --init --recursive
 
# using FLYCAST_VERSION = 170ae3477c5b69975540aa382ac9ce3442d5c40e
git checkout 170ae347
 
# apply patch 
patch -p1 < flycast.patch

# build
export PATH=$SDKTARGETSYSROOT/usr/bin:$PATH
source /opt/rk3399env.sh
CC_PREFIX=aarch64-linux- CFLAGS="-DTARGET_NO_OPENMP=1  -DMESA_EGL_NO_X11_HEADERS" LDFLAGS=-I$SDKTARGETSYSROOT/usr/include/EGL/  USE_SDL=1 USE_GLES=1  make -C shell/linux -f Makefile platform=arm64 -j4
```
- executable output:
	**reicast.elf** under **shell/linux**

## Note:
- How to fix this issue
> /opt/rk3399_toolchain/bin/../libexec/gcc/aarch64-buildroot-linux-gnu/8.4.0/cc1plus: error while loading shared libraries: libmpfr.so.4: cannot open shared object file: No such file or directory

-> Could be solved by creating a softlink for target
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpfr.so.6 /usr/lib/x86_64-linux-gnu/libmpfr.so.4
```
## Reference:
- refer to https://github.com/pac-women/ALP-toolchain for ALP toolchain environment
- refer to here: https://github.com/batocera-linux/batocera.linux/blob/95f96803e04027b7996c73ae05b40a891dad0ca9/package/batocera/emulators/flycast/flycast.mk for git commit with working Makefile
- the patched eglplatform.h comes from aarch64-buildroot-linux-gnu/sysroot/usr/include/EGL/eglplatform.h
