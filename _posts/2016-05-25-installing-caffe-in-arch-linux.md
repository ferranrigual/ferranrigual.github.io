---
layout: post
title:  "Installing Caffe in Arch Linux"
date:   2016-05-24 14:00:00
category: professional
tags: caffe arch
---

# Why Arch?

We are migrating our OS for development at [Sadako][sadako], from [Xubuntu][xubuntu] to [Arch Linux][arch]. Ubuntu is great when installing things because most of the developers test their software on this OS, but when compared to Arch, it lacks control over the system.

Arch is a minimalistic distribution, and that's what we were looking for. After only two weeks of use I know all the daemons running in my laptop. A clear advantadge if you don't want any unexpected errors conflicting with your software.

It has its counterpart too. It's way more technical than Ubuntu and it requires some initial learning, but I found its documentation very useful and the official forum has a lot of support from the community. 

# Caffe

[Caffe][caffe] is a deep learning framework. At Sadako we use it to train and test our neural network models.

Installing caffe in Arch shouldn't be difficult. Caffe is well documented and clearly specifies all of its dependencies. It is distributed in source and the user has to compile it himself. That's very useful for configurationn purposes, because offers the possibility to use or not the GPU and many other interesting but optional features.

# Installation process

I downloaded the caffe-master branch in zip format on 18/05/2016, from [its page][caffe-src] on GitHub. Most of the prerequisites where installed using `pacman`.

In Arch, some configurations have to be done before compiling Caffe:

- `OPENCV_VERSION := 3`
- `CUDA_DIR := /opt/cuda`
- Replace `/usr/lib/python2.7/dist-packages/numpy/core/include` with `/usr/lib/python2.7/site-packages/numpy/core/include` (dist-packages -> site-packages)

My first attempt was to configure Caffe to use only the CPU and not the GPU, the building process went straight forward.

# Problem #1: CUDA-7.5 vs GCC-6.1

When adding GPU support, I got the following error:

```
NVCC src/caffe/util/im2col.cu
/usr/lib/gcc/x86_64-pc-linux-gnu/6.1.1/include/stddef.h(436): error: identifier "nullptr" is undefined
/usr/lib/gcc/x86_64-pc-linux-gnu/6.1.1/include/stddef.h(436): error: expected a ";"
/usr/include/c++/6.1.1/x86_64-pc-linux-gnu/bits/c++config.h(202): error: expected a ";"
/usr/include/c++/6.1.1/exception(63): error: expected a ";"
[Ommited similar lines of error]
Error limit reached.
100 errors detected in the compilation of "/tmp/tmpxft_00004b9c_00000000-16_im2col.compute_50.cpp1.ii".
Compilation terminated.
Makefile:585: recipe for target '.build_release/cuda/src/caffe/util/im2col.o' failed
make: *** [.build_release/cuda/src/caffe/util/im2col.o] Error 1
```

This error is due to incompatibility issues between CUDA-7.5 and GCC-6.1. CUDA only supports GCC up to its 4.9 version (officially), but the CUDA compiler doesn't complain about GCC if you use newer versions up to the 5.2. Unfortunately, when using newer versions, errors like these appear. I could have tried to compile Caffe using an older version of GCC, but then there would be problems with all its dependencies, because most of them were installed by the system[^1]. 

[^1]: In Arch, like in most of the major linux distributions, the software is distributed using binary format, in packages. This means that every library and executable installed by the system has been compiled using the current version of GCC, which in Arch is GCC-6.1.1 at the moment of writing this. [Gentoo][gentoo] is a clear exception, all the software in Gentoo is distributed in code, and it is compiled by the user during the installation.

Fortunately, there is another solution. We can use one version of the compiler for the CUDA code, and another one for the rest of Caffe. Simply create a couple of links in the `bin` folder of CUDA, named `g++` and `gcc`, pointing to your desired version of the GCC binaries, and `nvcc` (the compiler of CUDA) will use them to compile its C code, instead of the ones in the system.

# Problem #2: ABI

At the beginning I did this using GCC-4.9, since it is the official supported version. But I got the following errors.

```
CXX/LD -o .build_release/tools/compute_image_mean.bin
.build_release/lib/libcaffe.so: undefined reference to `caffe::BlockingQueue<caffe::Batch<double>*>::pop(std::string const&)'
.build_release/lib/libcaffe.so: undefined reference to `google::base::CheckOpMessageBuilder::NewString()'
.build_release/lib/libcaffe.so: undefined reference to `google::protobuf::internal::NameOfEnum(google::protobuf::EnumDescriptor const*, int)'
.build_release/lib/libcaffe.so: undefined reference to `caffe::BlockingQueue<caffe::Batch<float>*>::pop(std::string const&)'
collect2: error: ld returned 1 exit status
Makefile:616: recipe for target '.build_release/tools/compute_image_mean.bin' failed
make: *** [.build_release/tools/compute_image_mean.bin] Error 1
```

This is different, now the syntax errors have disappeared, which is a very good signal. But the compiler still complains about missing symbols.

This was a headache. At first I checked in the code that every function was declared, and they were. I couldn't understand why the compiler wasn't finding its references. I tried more or less everything... from trying to install caffe using an [Arch package][caffe-git] from the AUR, to read as many forums on the internet as I could.

In an attempt to reduce the distance between the two GCC versions that I was using (at the end this is a compiling issue), I configured CUDA to use the GCC-5.2 version, like in Ubuntu-15.10. But the problem persisted. Fortunately, this was a step in the right direction.

To understand the problem, one must know about the [ABI][abi], the object code interface between user C++ code and the implementation-provided system and libraries. GCC-5.1 [can be configured][dualAbi] to use the old library ABI, or a new one conforming to the 2011 C++ standard. To configure it, I had to modify the following lines of the `Makefile`:

```diff
COMMON_FLAGS += -D_GLIBCXX_USE_CXX11_ABI=1
CXXFLAGS += -MMD -MP -std=c++11 -D_GLIBCXX_USE_CXX11_ABI=1
```

After using this configuration, the compilation finally succeeded.

[sadako]: http://www.sadako.es/
[xubuntu]: http://xubuntu.org/
[arch]: https://www.archlinux.org/
[caffe]: http://caffe.berkeleyvision.org/
[caffe-src]: https://github.com/BVLC/caffe
[gentoo]: https://www.gentoo.org/
[caffe-git]: https://aur.archlinux.org/packages/caffe-git/
[abi]: https://en.wikipedia.org/wiki/Application_binary_interface
[dualAbi]: https://gcc.gnu.org/onlinedocs/libstdc++/manual/using_dual_abi.html
