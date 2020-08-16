---
layout: post
author: cbtpro
---
webp是谷歌发布的开源图片格式

```shell
# 下载ffmpeg源码
git clone https://git.ffmpeg.org/ffmpeg.git
# 编译
cd ffmpeg

brew install fdk-aac&&brew install x264&&brew install x265&&brew install speex&&brew install pkg-config&&brew  install sdl2

./configure --prefix=/usr/local/ffmpeg --enable-gpl --enable-nonfree --enable-libfdk-aac --enable-libx264 --enable-libx265 --enable-filter=delogo --enable-debug --disable-optimizations --enable-libspeex --enable-videotoolbox --enable-shared --enable-pthreads --enable-version3 --enable-hardcoded-tables --cc=clang --host-cflags= --host-ldflags= --disable-x86asm --enable-libvpx --enable-libvorbis

make && make install
```

配置环境变量

vi ~/.bash_profile

```shell
export PATH=$PATH:/usr/local/ffmpeg/bin

export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig:/usr/lib/pkgconfig:/usr/local/Cellar/sdl2/2.0.8/lib/pkgconfig:/usr/local/ffmpeg/lib/pkgconfig
```



